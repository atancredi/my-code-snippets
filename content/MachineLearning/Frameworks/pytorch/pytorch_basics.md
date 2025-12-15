# Pytorch basics





## Evaluating the model

```python
# After training the model
mlp.eval()  # Set the model to evaluation mode

# Ensure x_test is a tensor and of the correct shape
x_test_tensor = torch.from_numpy(x_test).float()

# Make predictions
with torch.no_grad():  # Disable gradient calculation for inference
    y_pred = mlp(x_test_tensor)

# Convert predictions to numpy (if needed for further evaluation)
y_pred = y_pred.numpy()
```


## Save and Load model

```python
torch.save(mlp.state_dict(), 'mlp_model.pth')
```

```python
# Create a new instance of the model
mlp_loaded = MLP()

# Load the saved model weights
mlp_loaded.load_state_dict(torch.load('mlp_model.pth'))

# Set it to evaluation mode
mlp_loaded.eval()
```
