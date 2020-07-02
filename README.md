# Codebase for the KSE624 Final Project, Spring 2020 at KAIST

Authors: Rafikatiwi Nur Pujiarti, Willmer R. Quinones

Contact: rafikatiwi@kaist.ac.kr, wrafell@kaist.ac.kr

This is a practical implementation of [Jetbot AI Robot Kit (powered by NVIDIA Jetson Nano)](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetbot-ai-robot-kit/), developed for the **KSE624 2020 spring class (Mobile and Pervasive Computing for Knowledge Services)** at [KAIST](https://www.kaist.ac.kr/kr/), which responds to the following Problem Statement:

> Graduate Students living alone and off-campus needs to be aware of the condition outside before going out of their room because their plans, the way they dress, and the equipment they need to bring will depend on the condition outside.

To solve this problem, we developed a system in which the Jetbot scans the user's outfit using its camera, and tell the user (voice-based) what she is missing based on the weather condition outside. For instance, if it is cold, and the user is not wearing a jacket, the Jetbot would ask her to dress properly. We achieve this by building a deep neural network model trained with a subset of [Large-scale Fashion Database](http://mmlab.ie.cuhk.edu.hk/projects/DeepFashion.html).

## Applied Technologies
#### Google Cloud Platform
We use [Google Cloud Patform](https://cloud.google.com/text-to-speech/docs/reference/libraries) to handle the text-to-speech tasks.

#### OpenWeather API
The weather information is extracted using the [OpenWeather API](https://openweathermap.org/api).

#### AirVisual API
The air quality is obtained using the [AirVisual API](https://www.iqair.com/air-pollution-data-api).

#### Speech Recognition 
We use the [speech_recognition](https://www.codementor.io/@edwardzionsaji/simple-voice-enabled-chat-bot-in-python-kt2qi5oke) library from Python to handle the speech-to-text tasks.

#### User Recognition
We use the Jetbot camera and the [Microsoft Azure API for Object Detection](https://azure.microsoft.com/en-us/services/cognitive-services/computer-vision/) to detect the user and the an image of her in order to classify her outfit.



#### Outfit Classification
We use a deep convolutional neural network (ConvNet) model on [PyTorch](https://pytorch.org/), using [ResNet50](https://arxiv.org/abs/1512.03385) for the outfit recognition. Our model was trained using a subset of [Large-scale Fashion Database](http://mmlab.ie.cuhk.edu.hk/projects/DeepFashion.html): we manually cropped and extracted 5 categories from the whole database: **long pants, shorts, shirt, thin clothes,** and **thick coats** (700 images from each category). Then, we divided the dataset into training set (600 images) and testing set (100 images). You can get the dataset **[here](https://drive.google.com/file/d/1IdqY1mneqy3sb1bmKObyA9x1d2vAbByQ/view?usp=sharing)**.

To evaluate our model, we employed a 10-fold cross-validation method. The losses of the training process can be seen in the picture below. Our model achieved an accuracy of **0.81 ± 0.03** on our testing set.

![image](/images/evaluation_plot.png)

## System Flow

![image](/images/system_flow.jpg)

## Code Explanation

(1) **main.py**
- Initializes the Jetbot camera
- Waits for the user command
- Responds to the user

(2) **weather_callAPI.py**
- Collects and processes the weather and air quality information
- Filters the information that will be communicated to the user

(3) **clothes_recognition.py**
- Detects the user using the camera
- Classifies the user's outfit using the ConvNet model

(4) **jetbot_actions.py**
- Converts the user's speech to text
- Selects how the Jetbot should respond to the user
- Converts the text to speech

(Additional) **model_evaluation.ipynb**
- Train and validate the clothe classification model using k-fold cross-validation
- This code supposes that the dataset is ordered as follows:
```
+-- dataset
	+-- train
		+-- class 1
		+-- class 2
		...
		+-- class n
	+-- test
		+-- class 1
		+-- class 2
		...
		+-- class n
```
- You can get the dataset that we used **[here](https://drive.google.com/file/d/1IdqY1mneqy3sb1bmKObyA9x1d2vAbByQ/view?usp=sharing)**.

## Limitations and Future work
Our project is limited on the following
- Jetbot latency is considerably high, hence it might take couple of seconds to respond to the user's commands.
- *Mask recognition* was not employed to detect whether the user is wearing mask, so the Jetbot can alert the user when the air quality is not good.
- Given that umbrellas come in so many shapes, we did not develop an *umbrella detection model* for the Jetbot to alert the user that she is not bringing umbrella when it is raining.










