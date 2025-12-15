# Evaluate performance of regression models


```python
from sklearn.metrics import mean_squared_error, mean_absolute_error, r2_score

# Evaluate performance with various metrics
mse = mean_squared_error(y_test, y_pred)
mae = mean_absolute_error(y_test, y_pred)
r2 = r2_score(y_test, y_pred)

print(f"Mean Squared Error: {mse}")
print(f"Mean Absolute Error: {mae}")
print(f"R-squared: {r2}")
```

Lower MSE and MAE are better, and R² close to 1 means a good fit.