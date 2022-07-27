# **[2022 Synopsys ARC AIoT Design Contest](https://www.synopsys.com/zh-tw/taiwan/university-program/iot-initiative/2021-arc-aiot-design-contest.html)**
<!-- <a href="https://contest.synopsys.com.tw/2022ARC
" target="_blank"><img src="https://user-images.githubusercontent.com/96005167/181190562-62bdda9f-2405-4c3a-b0f7-1f53a0abd062.png"
alt="Link is failed!" width="300" border="10" /></a> -->
# **Contactless Control Panel**

## Team Info.
### NCTU Oasis Lab (Machine Learning Group)
<!-- ![](https://user-images.githubusercontent.com/96005167/181191742-5188627a-c551-4014-901f-e627fd9b6ce2.png) -->
<a href="[https://contest.synopsys.com.tw/2022AR](https://sites.google.com/a/g2.nctu.edu.tw/oasis_317a/)C
" target="_blank"><img src="https://user-images.githubusercontent.com/96005167/181191742-5188627a-c551-4014-901f-e627fd9b6ce2.png"
alt="Link is failed!" width="200" border="10" /></a>

#### Team ：  綠洲熊與他們的窩
#### Advisor：張錫嘉 教授
#### Members：陳冠瑋、鄭紹文、曹家輔、李家毓
#### Introduction:
We are students from the Oasis Lab of National Chiao Tung University (NCTU).

The research field of the lab covers hardware implementation of ML, ECC, and Security. We hope that the ideals and works will be turned into reality through this competition, and we will try our best to complete the project worth looking forward to!


## <font color="#4E2683">Outline</font>
### [1. Introduction](#作品概述)
### [2. 難點與創新](#難點與創新)
### [3. 設計與實現](#設計與實現)
### [4. 作品進度](#作品進度)
### [5. 測試結果](#測試結果)
### [6. 總結展望](#總結展望)

#  
## 作品概述

### <font color="#4E2683">Motivation</font>
* Smart building
* Smart security
* Prevent epidemic

### <font color="#4E2683">Solution</font>
We propose a contactless control panel combined with face authentication, which can send control signals and verify the user’s identity without touching the panel. 

* Using “Siamese Network” to train a face authentication model with a small number of examples, and deploy the model on ARC EM9D AIoT DK with TensorFlow Lite.
* Using Mediapipe Hand and OpenCV to implement a contactless virtual control panel through gesture recognition on NVIDIA Jetson Nano.

### <font color="#4E2683">Scenario 1: </font>Virtual elevator button panel + Visitor verification
*  Control the elevator with gesture recognition
*  Visitor verification
*  Virtual elevator control panel

### <font color="#4E2683">Scenario 2: </font>Smart door lock (example)
*  Identity verification
*  Stranger Record
*  Virtual touchless door lock

### <font color="#4E2683">Scenario 3: </font>Contactless ticket machine screen (example)
* Record the usage time of different users
* Combined with automatic recommendation system
* Avoid touching public touchscreens


## 難點與創新
### <font color="#4E2683">Innovation</font>
### - Using Siamese Network Architecture on TinyML
* Deep learning and CNN have made tremendous progress in the field of face recognition.
* We choose “ Siamese network” architecture and use its ability to discriminate similarity to judge the similarity of two different faces.  
* Compared with traditional CNN, the advantage of Siamese Network is that it does not need a large data set to train a good enough model, because the purpose of model is to compare the similarity of two inputs.

###  - Combining control panel with computer vision
* In the future, it can combined with AR glasses and widely used in the metaverse.

### <font color="#4E2683">Difficulties</font>
### - Model size problem
* Implementing a Siamese Network that can correctly identify faces in ARC EM9D AIoT DK is a difficult problem. Because ARC EM9D AIoT DK only has less than 2MB of storage to store weight. We tried to reduce the parameters, but if we reduce too many parameters, the verification result will be inaccurate.
* Therefore, how to balance the model size and validation accuracy is a major problem we face.

### - Activation op can not use
* In the original paper of Siamese Network, the sigmoid function is used as the activation function of the last layer.
* However, ARC EM9D does not support the sigmoid function as activation function.
* When we tried to use tanh as activation function with MSE as the loss function, we found that tanh did not support it either.
* So, we decided to change the layers and architecture to make it a brand new Siamese Network to comply with ARC EM9D op codes requirements.

## 設計與實現
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


