# Deep Learning in Physics

Trying to get the gist of it.


Deep learning works well on any project where a big amount of data is available to process. In photonics however a big amount of data is often expensive both computationally and economically.


DL is a data driven-approach: training the model means teaching him correlations in the dataset that allow to link the input to the output values, eventually converging to an empirical model describing the implicit rules behind the dataset.


Deep learning works well on any problem with lots of data: in many physics fields (as well as photonics) having lots of data is often expensive both economically and computationally.




# The artificial neural network

The basic building block of an Artificial Neural Network (ANN) is the artificial neuron, which is a mathematical function taking several input values. A weight parameter is associated with each of the inputs.

The neuron computes the sum of the weighted input values and then adds the value of an additional bias parameter. To this value, a so-called activation function $f$ is applied, and the resulting number is returned - also called the activation value of the neuron.
$$
output = f(\sigma_iI_i + B)
\\
I_i = \text{input $i$}
\\
\sigma_i = \text{weight of input $i$}
\\
B = \text{bias parameter}
$$

The key hypothesis is that the ANN is learning an hierarchy of features from the input data, where each layer is extracting a deeper level of characteristics. Thus, using many layers is crucial to ensure that a network has high abstraction capacity.


### linearity and non-linearity
If the model were made of only linear activation functions, it would be trivial to show that it can be identically represented by a single linear layer: the chain of two linear functions is still linear.

Nonlinear activation functions are used in deep ANN to perform hierarchical feature extraction.

Popular nonlinear activations are sigmoid (logistic activation function) and ReLU (rectified linear units). Sigmoid finds difficult to learn from samples far away from the bias, as the gradient for those samples is very small. ReLu is preferred because it has a constant gradient and is computationally cheap.

### inductive bias
When using a linear output activation, which can take arbitrary values (for example in regression tasks), the model should learn the data range along with the actual problem to solve.
An inductive bias, to help the network to focus on the essential learning task, must be introduced.

In regression tasks it can be introduced normalizing the output of the model with Sigmoid ($[0,1]$) or tanh activation ($[-1,1]$).


## Training

Training is the process where the network parameters are adapted to the dataset: the task is implicitly defined by the large dataset.

The learning process is done with a _loss_ function, which quantifies the network's prediction error (distance from the expected value). This function is optimized by modifying the weight and bias parameters of the network.

The model computes a batch of training samples in forward propagation to calculate the loss. The gradients of the loss function with respect to all network weights are calculate in a backpropagation step (using automatic differentiation - autodiff). The network parameters are finally adapted towards the negative gradient direction in order to minimize the loss function.

The learning rate is the size of the parameter modification steps, and it's a crucial parameter.
If the learning rate is too small the model takes too many iterations to reach the loss function minimum, and if it's too big it can miss the optimal solution.

### update step
This is the update step of the SGD (stochastic gradient descent) optimizer.
$$
\sigma_i = \sigma_i - lr \cdot \frac{\partial loss}{\partial\sigma_i}
$$
Other popular optimizers are _adam_ and _adamW_ (they often are faster to converge).


### what to check during training
During the training iterations (_epochs_), the loss curve should be monitored, expecting it to continuosly decrease.

When the loss decrease starts to stagnate, reducing the learning rate leads to further convergence.