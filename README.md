# **[2022 Synopsys ARC AIoT Design Contest](https://contest.synopsys.com.tw/2022ARC)**

<!-- <a href="https://contest.synopsys.com.tw/2022ARC
" target="_blank"><img src="https://user-images.githubusercontent.com/96005167/181190562-62bdda9f-2405-4c3a-b0f7-1f53a0abd062.png"
alt="Link is failed!" width="300" border="10" /></a> -->
<!-- # **Contactless Control Panel** -->

# Team Info.
### NCTU Oasis Lab (Machine Learning Group)
<!-- ![](https://user-images.githubusercontent.com/96005167/181191742-5188627a-c551-4014-901f-e627fd9b6ce2.png) -->
<a href="https://sites.google.com/a/g2.nctu.edu.tw/oasis_317a/
" target="_blank"><img src="https://user-images.githubusercontent.com/96005167/181191742-5188627a-c551-4014-901f-e627fd9b6ce2.png"
alt="Link is failed!" width="400" border="10" /></a>

### **非接觸式控制面板 - Contactless Control Panel**
### 綠洲熊與他們的窩
### Advisor: 張錫嘉 教授
### Members: 陳冠瑋、鄭紹文、曹家輔、李家毓
### Introduction:
We are students from the Oasis Lab of National Chiao Tung University (NCTU).

The research field of the lab covers hardware implementation of ML, ECC, and Security. We hope that the ideals and works will be turned into reality through this competition, and we will try our best to complete the project worth looking forward to!


# <font color="#4E2683">Outline</font>
### [1. Introduction](#introduction-1)
### [2. Difficulties & Innovation](#difficulties--innovation)
### [3. Design & Implementation](#設計與實現)
### [4. Demo & Result](#測試結果)
### [5. Conclusion](#總結展望)

 
# Introduction


<!-- https://user-images.githubusercontent.com/96005167/181204805-ef69177c-7cb1-4f42-ae35-33eff0ccf267.mp4 -->




### <font color="#4E2683">【Motivation】</font>
* Smart building
* Smart security
* Prevent epidemic

### <font color="#4E2683">【Solution】</font>
We propose a contactless control panel combined with face authentication, which can send control signals and verify the user’s identity without touching the panel ! 

* Using “Siamese Network” to train a face authentication model with a small number of examples, and deploy the model on ARC EM9D AIoT DK with TensorFlow Lite.
* Using Mediapipe Hand and OpenCV to implement a contactless virtual control panel through gesture recognition on NVIDIA Jetson Nano.

### <font color="#4E2683">【Scenario 1】: </font>Virtual elevator button panel + Visitor verification
*  Control the elevator with gesture recognition
*  Visitor verification
*  Virtual elevator control panel

![](https://user-images.githubusercontent.com/96005167/181206306-792f0f50-0c1e-4d1d-8ec2-fed1f45ca1ff.gif)

### <font color="#4E2683">【Scenario 2】: </font>Smart door lock 
*  Identity verification
*  Stranger Record
*  Virtual touchless door lock

### <font color="#4E2683">【Scenario 3】: </font>Contactless ticket machine screen
* Record the usage time of different users
* Combined with automatic recommendation system
* Avoid touching public touchscreens


# Difficulties & Innovation
### <font color="#4E2683">【Difficulties】</font>
#### - Model size
* Available memory size < 2MB
* Auto-Encoder: Dimension Reduction
* Post training quantization: fp32 → int8

#### - Input images pairs for the Siamese network
* Siamese network needs to compare two images at the same time
![image](https://user-images.githubusercontent.com/96005167/181212761-e70de490-c7af-4a76-9625-3cca4ffae1f4.png)

#### - Transmission time
* Validation data transmission
* Save images for comparison in the system memory
![image](https://user-images.githubusercontent.com/96005167/181213995-b7745e55-ff11-4a5d-a780-9d21a1a75de3.png)


#### - MLI Library does not support some Ops
* In the original paper of Siamese Network, the sigmoid function is used as the activation function of the last layer
* However, MLI Library does not support the sigmoid function as activation function
* When we tried to use tanh as activation function with MSE as the loss function, we found that tanh did not support it either
* When implement the L1 distance layer, int8 abs() op does not support too




### <font color="#4E2683">【Innovation】</font>
#### - Siamese Network Architecture on TinyML
 - Discriminate the similarity between two different input
 - Small number of samples are needed

#### - Small face verification model
 - Low power: Used in unreliable power supply scenarios 
 - Less computing & memory resources: Faster inference, faster deployment

#### - Input dimension reduction 
 - Auto-Encoder: Reduce the dimension of the input features
 - Solve the bottleneck of inference time
 - Deploy a more efficient model

####  - Combining control panel with computer vision
 - In the future, we hope that it can combined with AR glasses and widely used in the metaverse.


## Design & Implementation
### <font color="#4E2683">Deep Facial Verification </font>
* Deployment: ARC EM9D AIoT DK
* Task: Face verification 
* Model: Siamese Network Architecture
![](https://i.imgur.com/yQlpdwX.png)
![](https://i.imgur.com/Rkd7GjV.png)


### <font color="#4E2683">Face Detection</font>
* Deployment: ESP32 CAM
* Task: Face detection
* Model: MobileNet v2
![](https://i.imgur.com/Ve16loY.png)
![](https://i.imgur.com/Sjjd9Ct.png)

### <font color="#4E2683">Hand pose Estimation</font>
* Deployment: NVIDIA Jetson Nano
* Task: Gesture recognition, system control
* Model: MediaPipe Hand
![](https://i.imgur.com/kQZkbTl.png)
![](https://i.imgur.com/tdCANWX.png)

### <font color="#4E2683">Hardware Architecture</font>
![](https://i.imgur.com/YSK1XOf.png)

### <font color="#4E2683">Face Verification</font>
*  ARC EM9D AIoT DK
*  Siamese Network

### <font color="#4E2683">Siamese Network</font>
*  Siamese network inspired by “Siamese twins” which has a unique architecture
*  Classify if the two inputs are the same or different (learn the similarity)
*  Take two different inputs passed through two similar subnetworks 
*  The two subnetworks have the same architecture, parameters, and weights

### <font color="#4E2683">Why choose “Siamese Network”?</font>
*  Humans exhibits a strong ability to acquire and recognize new patterns
*  A few-shot classification model
*  Predict with just a single training example
### <font color="#4E2683">Siamese Network</font>
![](https://i.imgur.com/Dw48bRQ.png)

### <font color="#4E2683">Data preprocessing</font>

 
#### -Resize: 100x100
#### -Gray scale
#### -Data augmentation
* Rotation
* Flip
* Zoom
* Brightness
* Blur
* Noise
#### -Dataset
* Anchor: 600
* Positive: 600
* Negative: 600
* 
### <font color="#4E2683">Contactless Control Panel </font>
* NVIDIA Jetson Nano

### <font color="#4E2683">MediaPipe Hand Landmarks</font>

* The palm detector model is used to find the region of the palm from the image.
* The hand landmarks mod.el is used to mark the 3D key-points in the region of interest.
* Measure the distance between the index finger and the middle finger to achieve the click operation to select the buttons of virtual panel.
![](https://i.imgur.com/MVDNqC6.png)


## 作品進度
### <font color="#4E2683">Siamese Network (Orig.)</font> 
#### - Embedding subnetwork 
#### - Input size: (100,100,1)
#### - Output vector: (4096,1)
#### - Total params: 152,196,160
#### - Dataset:
*  anchor: 600 images
*  positive: 600 images
*  negative: 600 images
#### - Batch size: 16
#### - Loss function: 
* binary cross entropy 
![](https://i.imgur.com/Ay9nvju.png)
![](https://i.imgur.com/ahMRfLA.png)
### <font color="#4E2683">Siamese Network (Orig.)</font>
![](https://i.imgur.com/hXgd48x.png)
![](https://i.imgur.com/M7DdkFw.png)
![](https://i.imgur.com/BWff00f.png)

### <font color="#4E2683">Virtual Control Panel</font>
* NVIDIA Jetson Nano
### <font color="#4E2683">Program flow design</font>


**IDLE:**
- In the idle state, the system will stop performing gesture recognition, which can reduce the power consumption. 
- The ESP32-CAM and HC-SR04 are used as the trigger of the system. 
- If face or an object is detected, the system will enter the RECOGNIZING state. 

**RECOGNIZING:**
- In the RECOGNIZING state, the system will recognize the user’s gesture and communicate with the PC, and will continue to monitor the signal from the trigger.
- If it is not triggered for 15 seconds, the system will enter the IDLE state.
- If there is a trigger signal, it will reset the timmer.
![](https://i.imgur.com/p1pRgkU.png)
![](https://i.imgur.com/jVZg8aU.png)

### <font color="#4E2683">Simulation Animation </font>
* Use the Pygame library on the server to simulate the situation of the elevator. 
    * Going up and down
    * Opening and closing
* Send the result signal to the server through Wi-Fi to control the elevator simulation animation.
* In addition, we also consider the problem of elevator scheduling, so that the elevator can update the input floor in real time and can input multiple floors at the same time.
### <font color="#4E2683">Four sates of elevator control</font>
 
* MOVING: Moving state
* OPENING: Door opening state
* HOLD: Door holding state
* CLOSING: Door closing state


## 測試結果
### <font color="#4E2683">Siamese Network (Orig.)</font>
![](https://i.imgur.com/TugU36L.png)
### <font color="#4E2683">Simulation Animation </font>
* Demo Video
![](https://i.imgur.com/YaId3qU.png)


## 總結展望
### <font color="#4E2683">Hope to add… </font>
* Recording the time when strange visitors enter the building.
* Voice control 

### <font color="#4E2683">Hope it can be applied in the following scenarios:</font>
* Smart door lock
* Smart food ordering machine
* Smart automatic ticketing system
* ATM


