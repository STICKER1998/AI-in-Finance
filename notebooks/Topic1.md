Self - Attention
============================
The introduction is based on the paper ‘Attention is all you need'

# 1. Self - Attention
```
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
        self.query = nn.Linear(in_features=d_model, out_features=d_key, bias=False)
        self.key = nn.Linear(in_features=d_model, out_features=d_key, bias=False)
        self.value = nn.Linear(in_features=d_model, out_features=d_value, bias=False)

    def forward(self, x):
        """
        Parameters:
        - x (Tensor): Input tensor of shape (Batch_size, Sequence_length, d_model).
        Returns:
        - out (Tensor): Output tensor of shape (Batch_size, Sequence_length, d_value).
        """
        
        # Generate query, key, and value tensor
        queries = self.query(x)  # Shape: (Batch_size, Sequence_length, d_key)
        keys = self.key(x)       # Shape: (Batch_size, Sequence_length, d_key)
        values = self.value(x)   # Shape: (Batch_size, Sequence_length, d_value)

        # Calculate attention scores
        score = torch.einsum("bqd, bkd->bqk", [queries, keys])  # Shape: (Batch_size, Sequence_length, Sequence_length)

        # Apply softmax to normalize the scores
        attention = torch.softmax(score / (self.d_key ** 0.5), dim=2)

        # Weighted sum of values based on the attention scores
        out = torch.einsum("bqk, bkd->bqd", [attention, values])  # Shape: (Batch_size, Sequence_length, d_value)

        return out
        
```
