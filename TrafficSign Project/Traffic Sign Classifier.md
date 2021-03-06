# **Traffic Sign Recognition** 

## Writeup
---

**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./Pictures/Ds.png 
[image2]: ./Test_Signes/1.jpg 
[image3]: ./Test_Signes/2.jpg 
[image4]: ./Test_Signes/3.jpg 
[image5]: ./Test_Signes/4.jpg 
[image6]: ./Test_Signes/5.jpg 


## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf. You can use this template as a guide for writing the report. The submission includes the project code.

You're reading it! 
### Data Set Summary & Exploration

#### 1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

I used the pandas library to calculate summary statistics of the traffic
signs data set:

* The size of training set is 34799
* The size of the validation set is 4410
* The size of test set is 12630
* The shape of a traffic sign image is (32, 32, 3)
* The number of unique classes/labels in the data set is 42

#### 2. Include an exploratory visualization of the dataset.

Here is an exploratory visualization of the data set. It is a bar chart showing amount of training examples for each traffic sign

![image1]

### Design and Test a Model Architecture

#### 1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

I try some different technics for data preprocessing, but only 2 show better accuracy of model. 

So I turn color pictures to gray. Simply by multiplying red green and blue channels by coeeficients 0.299, 0.587, 0.114 respectively. Benefits from this method is reducing number of input patameters from 32x32x3 to 32x32. After this operation accuracy gone better.

And used sklearn.preprocessing.normalize function to move dataset mean to zero. This funxtion worked little bit better than (x-125)/125. This step will help optimiser to do it's job. I've check this by comparing accuracy of model before and after.

#### 2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

My final model consisted of the following layers: I get LeNet model as base and add drop outs in 2 steps to avoid overfitting. I played with different activation functions like sigmoid and ELU but accuracy was really bad, so i stay with RELU. 

| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x1 Gray image   							| 
| Convolution 3x3     	| 5x5 stride, valid padding, outputs 28x28x6 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 14x14x6 	     			|
| Convolution 3x3     	| 5x5 stride, valid padding, outputs 10x10x16 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 5x5x16 	     			|
| Flaten                | outputs 400   								|
| dropuot       		| k_prob = 0.5									|
| Fully Connected		| Output = 120     								|
| RELU					|												|
|Fully Connected		| Output = 84           				       	|
| RELU					|												|
| dropuot       		| k_prob = 0.5									|
| Fully Connected		| Output = 43          				        	|

#### 3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

To train the model, I used an following paramerers:
EPOCHS = 50
optimiser  = AdamOptimizer
learn rate = 0.001
keep_prob_value = 0.5

#### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:
* training set accuracy of = 0.996
* validation set accuracy of = 0.954
* test set accuracy of = 0.942

If an iterative approach was chosen:
* What was the first architecture that was tried and why was it chosen? I start from LeNet architecture, just tune it to color images. It was good start because it work well for 0-9 signs.
* What were some problems with the initial architecture? Main problem was bad accuracy on test set.
* How was the architecture adjusted and why was it adjusted? Typical adjustments could include choosing a different model architecture, adding or taking away layers (pooling, dropout, convolution, etc), using an activation function or changing the activation function. One common justification for adjusting an architecture would be due to overfitting or underfitting. A high accuracy on the training set but low accuracy on the validation set indicates over fitting; a low accuracy on both sets indicates under fitting. I Play with different activation functions, but results was bad, so after I increase number of epochs model looks to overfit, fased big differens between validation accuracy and test one. After that I add some dropouts to avoid this.
* Which parameters were tuned? How were they adjusted and why? Increased number of epochs, and add keep_prob parameter.
* What are some of the important design choices and why were they chosen? Dropout layers were added.

If a well known architecture was chosen:
* What architecture was chosen? 
* Why did you believe it would be relevant to the traffic sign application? I shows good accuracy and not big difference between test and validation set.
* How does the final model's accuracy on the training, validation and test set provide evidence that the model is working well? We get close results so model fited well (not over and not underfited)
 

### Test a Model on New Images

#### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are five German traffic signs that I found on the web:

![image2] ![image3] ![image4] 
![image5]![image6]

2 of this pictures was hard to classify because there is few examples in test set. To avoid this we should train our model on more data. All this images has high resolution, they was resised to 32x32 using  skimage.transform.resize function. Signs after resizing has good contrast and little blurriness. So I think the reason of misclassifying 100 speed Turn right ahead  and limit was low volume of train set for this.

#### 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction:

| Image			        |     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| Right-of-way at the next intersection    		| Right-of-way at the next intersection  			| 
| Stop   		    	|Stop  										    |
| Speed limit (100km/h)	| Speed limit (30km/h)							|
| Turn right ahead      |No vehicles					 				|
| Priority road         | Priority road  							    |


The model was able to correctly guess 3 of the 5 traffic signs, which gives an accuracy of 60%.  Accuracy 0.6 is bellow of test accuracy 0.942 so model has issues and need to improve.
#### 3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

The code for making predictions on my final model is located in the 15th cell of the Ipython notebook.

For the first image, the model is relatively sure that this is a Right-of-way at the next intersection  sign (probability of 0.996), and the image does contain a Right-of-way at the next intersection sign. The top 2 soft max probabilities were
 (other are close to 0).
 
| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| .996        			|  Right-of-way at the next intersection  	    | 
| .004    				| Beware of ice/snow							|


For the first image, the model is relatively sure that this is as top  sign (probability of 0.998), and the image does contain a stop sign. The top 2 soft max probabilities were (other are close to 0).

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| .998        			|  Stop                                         |
| .002     				| Keep right									|



