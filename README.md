AI in Finance
==============================
## Topics
|Topics           |         Link          |
| ---             |---                    |
|Self - Attention |[Topic 1](./notebooks/Topic1.md) |
|Transformer      |    coming soon        |
|Informer         |    coming soon        |


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
