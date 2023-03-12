# Humanoid_Monitoring_Robot
## Introduction
A humanoid monitoring robot that is designed to resemble humans in form, movement, performance and functions. Is equipped with cameras, servo motors, rechargeable batteries and other devices. These robots are typically used for surveillance, monitoring, and data collection purposes in areas where it may not be safe or feasible for humans to enter, such as hazardous environments or disaster zones. They can also be programmed to perform various tasks, such as detecting and tracking human activity, providing assistance in emergency situations, and even providing basic care in healthcare and elderly care settings. The development of humanoid monitoring robots is a rapidly evolving field, with advancements in technology continuously improving their capabilities and potential applications.
## Components
•	ESP32 cam 
•	Pan Tilt Servo assembly 
•	SG90 servo motors 
•	L298N motor driver module 
•	7-12 V Rechargeable battery 
•	Ardunio uno
•	Jumper wires
## Hardware and software configurations 
## Step # 1:
Download and install Arduino ide through following link
https://filehippo.com/download_arduino/1.8.16/
## Step # 2:
Connect the Esp32 cam with Arduino uno by following the diagram
 
## Step # 3:
Open the code file “Camera_Car_with_PanTilt_Control.ino” in Arduino ide and install esp32cam board in Arduino Ide by following the link 
https://randomnerdtutorials.com/installing-the-esp32-board-in-arduino-ide-windows-instructions/
## Step # 4:
Do some setting in Arduino IDE by following the screenshot
 
## Step # 5:
Upload the code and integrate the hardware according to the following diagram:






 


## Functionality
This is a WI-FI controlled humanoid robot. It can be controlled using a mobile app. This mobile app can be open by entering an IP address (192.168.4.1) on the browser. Sometimes we might have to reconnect the WI-FI if app does not open. There are four buttons that handles the movement of the car left, right, backward and forward. Then there are buttons for the speed and light control. And we also have button PAN and TILT button. This PAN Button will handle the camera movement in left and right direction. However, TILT Button will handle the camera movement in upward and downward direction. The light button will be used to turn the flash light on and off. Moreover we can increase and decrease the intensity of this flash. The speed button can be used to control the speed of video streaming.
  
