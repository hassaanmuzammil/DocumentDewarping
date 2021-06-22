# DocumentDewarping
A combination of deep learning using squeeze-net and geometric transformation.

Introduction: 
Document dewarping is a method to automatically flatten a warped document. 
In this repo, I used a combination of pytorch implemented squeeze-net model and homography techniques to correct a scanned document. 
Hence, the requirements of the project can be thought as two-fold; corners/edge detection using deep learning and image reconstruction using geometric transformations.

Objectives:
- Training a squeeze-net model to detect 4 corners of a document.
- Using the 4 corners to transform and fix the image.

Deep Learning for Corner Detection: 
I used pytorch squeeze-net model with pretrained weights because of its pythonic interface. Also, this is a development environment test and not a production test, so pytorch seemed appropiate. An additional fully connected layer was added so that we can have 8 labels at the output 
corresponding to the x,y coordinates of four corners in the sequence topleft, topright, bottomright, bottomleft. Thus, this was a problem of transfer learning and I only
had to train the weights of the fully connected layer, which had about 8000 parameters. 

Dataset: 
The dataset used for this purpose is the synthetic dataset created by Synthetic Dataset created by S.A Abbas and S.ul.Hussan. 
https://drive.google.com/open?id=0B0ZBpkjFckxyNms0Smp0RWFsdTQ
This is stored in a .mat file extension, with separate train files for labels and images. The examples are 6615 in number. All of which were used to train the model.
After training the model with 10 epochs, 0.01 learning rate, 16 batch size and using an MSE loss function, the train error was significantly reduced. However, it can be reduced even further with further learning.
Then, we were able to predict the four corners by a simple forward pass and these were used in the next part of geometric transformation.

Geometric Transformation:
The common technique of homography was used which requires four corresponding points. Hence, the four corners were passed from the model output to this problem. These 
points were mapped at the corners of the new image. Homography matrix was calculated and every coordinate/pixel of the original image was transformed. cv2 algorithms were used for this purpose.

Results:
The train MSE reduced from to an average of less than 15 per example, showing significant learning. The algorithm was tested using a training set image and a random image taken from a phone camera. Satisfactory results were produced.  

References:
- https://blog.tensorflow.org/2019/12/kingsoft-wps-document-image-dewarping.html
- https://github.com/thomasjhuang/deep-learning-for-document-dewarping
- https://pytorch.org/hub/pytorch_vision_squeezenet/
- https://khurramjaved96.github.io/RecursiveCNN.pdf
