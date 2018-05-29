This is a brief summary of the SVHN benchmark project.

Pre-processing the data

The dataset is a set of 34,168 RGB images, containing one or more digits in them. The labels for all the images are provided. Also, bounding boxes are provided in the form of X and Y co-ordinates for every digit in every image.
Pre-processing included three steps:
  1) Cropping the image discarding the region outside the bounding boxes.
  2) Resizing the image to a 64x64 resolution.
  3) Kernel sharpening using a 3x3 matrix : [[-1,-1,-1],[-1,-9,-1],[-1,-1,-1]]

Network Architecture

Conv1 : 5x5x3 convolution 32 filters, zero-padding : yes (done for all conv layers),
Activation : ReLU
max_pool (2x2)

Conv2 : 5x5x3 convolution 64 filters,
Activation : ReLU
max_pool (2x2)

Conv3 : 5x5x3 convolution 128 filters,
Activation : ReLU
max_pool (2x2)

Conv4 : 5x5x3 convolution 200 filters,
Activation : ReLU
max_pool (2x2)

Conv5 : 3x3x3 convolution 300 filters,
Activation : ReLU
max_pool (2x2)

Dense layer : 1024 units,
Activation : ReLU
dropout : 0.5

Readout layer : 5 units (Assuming a maximum of 5 digits per image)
Activation : Softmax

Optimizer used for training : Adam, with learning rate 10^(-4)
We used Adam instead of the usual batch gradient descent, because the learning rate changes as you train - it auto-estimates the momentum factor for training from two moment values calculated from the cost and the change in weights.

Training
Trained for 38 epochs on a NVIDIA 940M GPU. Time taken was ~100 minutes.

Results

Initial loss value : 7.12

Final loss value : less than 0.1

Accuracy on the test set : 79.66%


How to run this on my machine?


Read the images whose digits are to be recognised using the cv2.imread() function.
Download the checkpoint files and the weights file, and save them.
In the main function, create an object of the SVHN class using the load_model method:

  obj = SVHN.load_model(name = '/address of the checkpoint and weights files/') 
  
  obj.get_sequence(image you read using cv2)
  
This should print the result to the terminal.
