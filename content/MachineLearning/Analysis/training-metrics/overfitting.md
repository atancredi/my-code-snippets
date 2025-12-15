# some notes on overfitting models in training

### red flags:
- when training loss goes down and validation loss goes up
- very high accuracy on training set, lower or lowering validation accuracy
    - a big (and growing) gap between accuracies on training and validation sets is suspicious.

### what is happening:
- model is learning noise and details, while losing generality.