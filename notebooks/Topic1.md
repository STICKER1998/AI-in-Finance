
Topic 1： Attention
============================
The introduction is based on the paper [‘Attention is all you need'](https://arxiv.org/abs/1706.03762)

# 1. Self - Attention
```python
class SelfAttention1H(nn.Module):
    def __init__(self, d_model, d_key, d_value):
        """
        Initialize the self-attention module.

        Parameters:
        - d_model (int): Dimensionality of the input feature space.
        - d_key (int): Dimensionality of the key and query vectors. 
        - d_value (int): Dimensionality of the value vectors.
        """
        super(SelfAttention1H, self).__init__()
        self.d_model = d_model
        self.d_key = d_key
        self.d_value = d_value

        # Linear transformations for query, key, and value vectors
        self.W_Q = nn.Linear(in_features=d_model, out_features=d_key, bias=False)
        self.W_K = nn.Linear(in_features=d_model, out_features=d_key, bias=False)
        self.W_V = nn.Linear(in_features=d_model, out_features=d_value, bias=False)

    def forward(self, x):
        """
        Parameters:
        - x (Tensor): Input tensor of shape (Batch_size, Sequence_length, d_model).
        Returns:
        - out (Tensor): Output tensor of shape (Batch_size, Sequence_length, d_value).
        """
        
        # Generate query, key, and value tensor
        queries = self.W_Q(x)  # Shape: (Batch_size, Sequence_length, d_key)
        keys = self.W_K(x)       # Shape: (Batch_size, Sequence_length, d_key)
        values = self.W_V(x)   # Shape: (Batch_size, Sequence_length, d_value)

        # Calculate attention scores
        score = torch.einsum("bqd, bkd->bqk", [queries, keys])  # Shape: (Batch_size, Sequence_length, Sequence_length)

        # Apply softmax to normalize the scores
        attention = torch.softmax(score / (self.d_key ** 0.5), dim=-1)

        # Weighted sum of values based on the attention scores
        out = torch.einsum("bqk, bkd->bqd", [attention, values])  # Shape: (Batch_size, Sequence_length, d_value)

        return out
        
```

# 2. Multi-Head Attention
```python
class SelfAttention(nn.Module):
    def __init__(self, d_model, heads_num):
        """
        Parameters:
        - d_model (int): Total dimensionality of the input feature space.
        - heads_num (int): Number of attention heads.

        The input feature space is divided equally across all heads.
        """
        super(SelfAttention, self).__init__()
        self.d_model = d_model
        self.heads_dim = d_model // heads_num  # Dimensionality of each head
        
        assert (d_model % heads_num == 0), "d_model should be divisible by the number of heads"

        # Create a list of self-attention heads
        self.heads = nn.ModuleList([
            SelfAttention1H(d_model=d_model, d_key=self.heads_dim, d_value=self.heads_dim)
            for _ in range(heads_num)]
        )

        # Linear layer for projecting concatenated output of all heads back to the original dimension
        self.W_O = nn.Linear(in_features=self.heads_dim * heads_num, out_features=d_model)

    def forward(self, x):
        """
        Parameters:
        - x (Tensor): Input tensor of shape (Batch_size, Sequence_length, d_model).
        Returns:
        - Tensor: Output tensor of shape (Batch_size, Sequence_length, d_model).
        """
        # Concatenate the output of each head
        out = torch.concat([head(x) for head in self.heads], dim=-1)  # Shape: (Batch_size, Sequence_length, d_model)

        # Debugging prints: uncomment to see the shape of each head's output
        # for head in self.heads:
        #     print(head(x).shape)

        # Project the concatenated output back to the original dimensionality
        return self.W_O(out)
```

# 3. ProbSparse Attention
```python
class ProbAttention(nn.Module):
    def __init__(self, factor = 5, scale = None, attention_dropout=0.1, output_attention = False):
        super(ProbAttention, self).__init__()
        self.factor = factor
        self.scale = scale
        self.output_attention = output_attention
        self.dropout = nn.Dropout(attention_dropout)
    
    def _prob_QK(self, Q, K, sample_K, n_top):
        # 计算有效的n_top个scores
        B, H, L_K, E = K.shape # shape: (Batch_size, Head_num, length of Keys, dimension)
        _, _, L_Q, _ = Q.shape # shape: (Batch_size, Head_num, length of Queries, dimension)
        
        # Calculate the sampled Q_K
        ## 构造L_Q份(L_K,E)的Keys矩阵
        K_expand = K.unsqueeze(-3).expand(B, H, L_Q, L_K, E)
        print("K_expand:", K_expand.shape)
        # shape: (Batch_size, Head_num, length of queries, length of keys, dimension)
        
        index_sample = torch.randint(L_K, (L_Q, sample_K))
        print("index_sample:", index_sample.shape)
        
        K_sample = K_expand[:,:,torch.arange(L_Q).unsqueeze(1),index_sample,:]
        # shape: (Batch_size, Head_num, length of queries, sample_k, dimension)
        print("K_sample:", K_sample.shape)
        
        Q_K_sample = torch.einsum("bhije, bhkle-> bhijl",[Q.unsqueeze(-2), K_sample]).squeeze(-2)
        # shape: (Batch_size, Head_num, length of queries, sample_k)
        print("Q_K_sample:", Q_K_sample.shape)
        
        
        # Find the Top_k query with sparisty measurement
        M = Q_K_sample.max(-1)[0] - torch.div(Q_K_sample.sum(-1), L_K)
        # shape: (Batch_size, Head_num, length of queries)
        print("M:", M.shape)
        M_top = M.topk(n_top, sorted = False)[1]
        # shape: (Batch_size, Head_num, n_top)
        print("M_top:", M_top.shape)
        
        # Use the reduced Q to calculate Q_K
        Q_reduce = Q[torch.arange(B)[:,None,None], torch.arange(H)[None,:, None], M_top, :]
        # shape: (Batch_size, Head_num, M_top, dimension)
        print("Q_reduce:", Q_reduce.shape)
        
        Q_K = torch.matmul(Q_reduce, K.transpose(-2, -1))
        # shape: (Batch_size, Head_num, M_top, length of keys)
        print("Q_K:", Q_K.shape)
        
        return Q_K, M_top
    
    def _get_intial_context(self, V, L_Q):
        # 先初始化输出为V的均值
        B, H, L_V, D = V.shape
        # shape: (Batch_size, Head_num, length of Values, dimension)
        V_sum = V.mean(-2)
        # shape: (Batch_size, Head_num, dimension)
        print("V_sum:", V_sum.shape)
        context = V_sum.unsqueeze(-2).expand(B, H, L_Q, V_sum.shape[-1]).clone()
        # shape: (Batch_size, Head_num, length of Queries, dimension)
        print("context_init:", context.shape)
        
        return context
    
    def _update_context(self, context_in, V, scores, index, L_Q):
        # 更新index位置处的V的值
        B, H, L_V, D = V.shape
        
        attn = torch.softmax(scores, dim=-1)
        
        context_in[torch.arange(B)[:,None,None], torch.arange(H)[None,:, None], index, :] = torch.matmul(attn, V).type_as(context_in)
        
        return context_in
    
    def forward(self, queries, keys, values):
        B, L_Q, H, D = queries.shape
        _, L_K, _, _ = keys.shape
        
        queries = queries.transpose(2,1)
        keys = keys.transpose(2,1)
        values = values.transpose(2,1)
        print("Adjusted shape for Q,K,V:",queries.shape)
        
        
        U_part = self.factor * np.ceil(np.log(L_K)).astype('int').item()
        u = self.factor * np.ceil(np.log(L_Q)).astype('int').item()
        
        U_part = U_part if U_part<L_K else L_K
        u = u if u<L_Q else L_Q
        
        scores_top, index = self._prob_QK(queries, keys, sample_K = U_part, n_top = u)
        
        context = self._get_intial_context(values, L_Q)
        context = self._update_context(context, values, scores_top, index, L_Q)
        
        return context.transpose(2,1).contiguous()
```
各变量的形状如下：
```python
Initial shape for Q,K,V: torch.Size([32, 96, 8, 64])
Adjusted shape for Q,K,V: torch.Size([32, 8, 96, 64])
K_expand: torch.Size([32, 8, 96, 96, 64])
index_sample: torch.Size([96, 25])
K_sample: torch.Size([32, 8, 96, 25, 64])
Q_K_sample: torch.Size([32, 8, 96, 25])
M: torch.Size([32, 8, 96])
M_top: torch.Size([32, 8, 25])
Q_reduce: torch.Size([32, 8, 25, 64])
Q_K: torch.Size([32, 8, 25, 96])
V_sum: torch.Size([32, 8, 64])
context_init: torch.Size([32, 8, 96, 64])
Final Context: torch.Size([32, 96, 8, 64])
```
