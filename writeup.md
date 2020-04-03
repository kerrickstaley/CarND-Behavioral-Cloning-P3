# **Behavioral Cloning** 

### Project Summary
In this project I trained a neural network to drive a simulated car around a track. The neural network I created
controls the car's steering; the throttle/brake code was provided for me.

The code for this project is in [project.ipynb](project.ipynb). A rendered version of the notebook after I ran it is [here](http://htmlpreview.github.io/?https://github.com/kerrickstaley/CarND-Behavioral-Cloning-P3/blob/master/project_rendered.html).

You can see a video of the car driving around the simulated track by clicking below:
[![image preview did not load :(](https://img.youtube.com/vi/0oLjrkQsq6o/0.jpg)](https://www.youtube.com/watch?v=0oLjrkQsq6o)

[Here is the trained Keras model](https://github.com/kerrickstaley/CarND-Behavioral-Cloning-P3/raw/master/model.h5).

### Model Architecture and Training Strategy

I used a convolutional neural network with the architecture described in
[this Nvidia article](https://devblogs.nvidia.com/parallelforall/deep-learning-self-driving-cars/). A graphic from that
article is below:

<img src="https://devblogs.nvidia.com/parallelforall/wp-content/uploads/2016/08/cnn-architecture.png" width="300">

The network starts with three convolutional layers with 5x5 filters and 2x2 strides. The first layer has 24 output features,
the second has 36, and the third has 48. Then there are two more convolutional layers, both with 3x3 filters,
1x1 strides, and 64 output features. Afterwards there are 5 fully connected layers with 1164, 100, 50, 10, and 1
neurons. The last layer is fed directly to the car's steering control without applying an activation function.

In order to reduce overfitting, I applied dropout on the dense layers with 1164, 100, and 50 neurons.

To train the model, I used the Adam optimizer with all the default parameters from the
[original paper](https://arxiv.org/abs/1412.6980v8). I kept 20% of the test data as a holdout for validation.

I created the training data by manually driving the car around the virtual track, and recording images from the
car's simulated front, left, and right cameras, as well as the steering angle. I recorded 3 laps in the
counter-clockwise direction, 2 laps in the clockwise direction, and 1 lap of zig-zagging on and off towards and away
from the edge of the road. For the last lap, only data from when the car was returning to the center of the road was
used. Since the test environment only uses the center camera, I adjusted the steering angle associated with the
left/right camera images by 7.5 degrees. For example, if the training data recorded a 10 degree rightward turn, an angle
of 17.5 degrees rightward would be be associated with the left camera image in the data fed to the neural network.

As I developed my model, the biggest improvements came from collecting more training data, using a more sophisticated
model, and applying dropout more aggressively (originally I only applied it to the 1164 and 100 neuron layers, but I
got better results also applying it to the 50 neuron fully connected layer).
