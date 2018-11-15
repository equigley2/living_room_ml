<h1> Living Room </h1>

<h6>Emily Quigley</h6>  

<h3>Premise:</h3> Part of my hobby, flipping houses, is to tear out an outdated room and build it back from scratch. Design becomes really important because having a well designed home will increase your profits when you sell. It can be really time consuming to develop the new design with fresh colors and materials and also hard to envision when you are staring at a room torn down to the studs.

<h3>My Question: What if there was machine learning for that? </h3>

<h3>Phase 1:</h3> Take a photo of an outdated living room and train a model to segment the sofa. This will later be used to replace with a new sofa which will provide the "redesign" element.
<br>
<br>
![] img ='segnetarch.png'

<h6> Data Set:</h6> ADE20K Living Room Images
This dataset came with an original image and a segmented image for each item in the room. Upon diving into the dataset I discovered that while the photos looked like each object was colored according to a specific code, they were not the same RGB values. I knew I was going to have to do a significant amount of cleaning on my data to make these images work.

<h3>Downsize:</h3> Working with images can be computationally expensive so for the purpose of this project I have chosen to focus solely on the sofa by making sofa pixels white and making all other pixels black.

<h3>Creating Labels</h3>


<h3>Complications</h3>
OpenCV can be a love, hate relationship. It's very technical to work with but also provides more advanced functionality.
<br>
<br>
Instead of RGB, OpenCV uses BGR so I discovered the hard way, if you are using OpenCV only use OpenCV for image processing.
Another complication is with resizing. I had used resize which was SKimage's resize instead of cv2.resize. I was trying to use some advanced function later on and was getting error messages mentioning CV_8U. I determined it wanted 8 bytes so I started checking the bytes in different stages and found out that after resizing I was getting a type float64. When I switched to cv.2 resize it solved this problem.
<br>
<br>
Later on I wanted to find the unique pixel colors and saw I was now getting 1105 unique pixel colors whereas previously I had gotten right around 35. I traced this back line by line to the resize function. I added the nearest neighbors interpolation method to solve this problem. Interpolation is a technique that helps handle distortion when resizing.

![] img='interpolation.png'

<h3>Solution</h3>
1. For each image, find the unique RGB values
2. Argsort and a mask to sort the RGB values from highest frequency to lowest
3. Make a mask for each unique RGB value in order to display an object.


<h3> CNN Modeling </h3>



1. 5 hidden layers
2. Each layer used padding
3. Optimizer used was Adadelta
4. Every activation was ReLU except the end was Softmax

One issue we encountered was downsampling beyond a 1 X 1 image.
<br>
We poured significant effort into perfecting a Convolutional Neural Network.  After numerous errors Chris perfected the network.

<img src="10Images.png">
<img src="history.png">
<img src="full_model_acc_loss.png">

<h3> VGG16  Modeling </h3>
<img src="vgg_macroarchitecture.png">
1. VGG is  pretrained model so all of the inner workings you see in the photo, we aren't able to make changes
2. VGG 16 indicates 16 weight layers
<!-- <img src="vgg_macroarchitecture.png" alt="Smiley face" height="100" width="100"> -->
<br>
3. First, we ran through one batch of 10,00 to test on a smaller segment of photos
4. Model clearly did not perform well because the true label wasn't in our top 3 predictions
5. We assume our images didn't classify well due to the low resolution

<img src="percentages.png" height="500">

<img src="value3.png">
<img src="value6.png">
<img src="value7.png">
<img src="value9.png">
<img src="value1.png">
<img src="value2.png">
<img src="value14.png">
<img src="value16.png">

<h3> Complications </h3>
* Data organization issues
* Naming issues
* Sizing the images after pooling, used padding to solve
* Initial model was guessing 10%
* Data sizing issues to make batches and then resize larger for the VGG model

<h3> Learnings </h3>
* Be patient, models take time to improve,  if you don't wait long enough you might change something pre-emptively

<h3> Future Work </h3>
* Build and perfect a model the night before.
* Understand shapes
* Drop $2500 for high-end computing power (stand up Amazon account sooner)
* Transfer Learning
<br>
* Test with new images from online
* Use better resolution photos to improve our model
* Test with VGG19
# living_room_ml
