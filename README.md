# TrafficLightSubmission
# **Traffic Sign Classification**

## Huy Nguyen

---

**Traffic Sign Classification**

[//]: # (Image References)
[image_80]: ./80.jpg "80"
[image_STOP]: ./STOP.jpg "STOP"
[image_arrowup]: ./arrowup.jpg "Ahead only"
[image_noentry]: ./noentry.jpg "No Entry"
[image_roundaboutmandatory]: ./roundaboutmandatory.jpg "Roundabout Mandatory"
[image_slipery]: ./slipery.jpg "Slippery"

[image_speed50]: ./speed50.png "Speed limit 50"
[image_speed60]: ./speed60.png "Speed limit 60"
[image_speed70]: ./speed70.png "Speed limit 70"
[image_end]: ./end.png "End no passing"
[image_left]: ./left.png "Left turn ahead"
---\

## 1. File submitted.

## 2. Dataset exploration.
**Dataset summary**
Images in the dataset have shape (32,32,3) (RGB).
There 3 sets:
Training set: 34799 samples
Validation Set: 4410 samples
Test Set: 12630 samples

There are 43 labels.

**Exploratory Visualization**
Some samples:

![image_speed50]
![image_speed60]
![image_speed70]
![image_end]
![image_left]

### 3. Design and Test a Model Architecture

**Preprocessing**
It can be noticed that, for many signs, the shapes/edges in the signs can be very good signals. Therefore, before feeding the image into the model, I compute the grayscale of the images and stack it on top of the images to create an extra channel (i.e., 4 channel in total).

I also normalize the data using:
x = x / 255 - 0.5

**Model Architecture**
The model was inspired by LeNet. Here are some changes that I made on top of LeNet:
1. I increase the number of filters for the first and second convolutional layers from 6 and 16 respectively to 32 and 64 respectively.
2. I add an extra/third layer of convolutional layer that output shape is 2x2x128. This reduce the size of the flatten output (from 1600 to 512).
3. For the fully connected layers, I changes the output size to 512 -> 256 -> 128 -> 43.
4. As the new model now is much large than the original LeNet, there is a risk of overfitting. To avoid this, I add dropout layer (with keep_prob=0.5 ) after each of the convolutional and fully connected layer.

**Model Training**
During the model training, these are the set of parameter that I tuned during training:
1. EPOCHS: I find that setting EPOCHS to greater or equal than 15 should be sufficient. I do see the validation accuracy increase when increasing the EPOCHS to more than 50 but it does not seem to impact the test accuracy much.
2. BATCH_SIZE: I tested with different values of BATCH_SIZE: 128, 256, 80, 50, 64. I found that 64 seems to be the best value.
3. learning_rate: I tried 0.01, 0.001, 0.0005, 0.0001. I find that 0.0005 and 0.001 give best results. I pick 0.0005 as it seems to be slightly better.
4. keep_prob: I tried 0.95, 0.9. 0.85, 0.8, 0.75, 0.7, 0.6, 0.5. I found that 0.7 seems to give me the best balance between high validation accuracy and keep the model from being overfitting.

**Solution Approach**
I use the same cost function, accuracy function and optimizer algorithm that were used in the lab. I can get the validation and test accuracy to 0.93 or greater. (See html).

### 4. Test a Model on New Images

**Acquiring New Images**
I found 6 of these signs from Google Images:
![image_80]
![image_STOP]
![image_arrowup]
![image_noentry]
![image_roundaboutmandatory]
![image_slipery]

**Performance on New Images**
The trained model detected 5 out of 6 signs correctly.

**Model Certainty - Softmax Probabilities**

![image_80]
 - Softmax probability: 0.05

![image_STOP]
- Softmax probability: 1.00

![image_arrowup]
- Softmax probability: 1.00

![image_noentry]
- Softmax probability: 1.00

![image_roundaboutmandatory]
- Softmax probability: 0.9999

![image_slipery]
- Softmax probability: 1.00
