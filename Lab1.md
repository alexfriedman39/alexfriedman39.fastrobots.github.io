# Lab 1 

## Lab 1A: The Artemis Board

The purpose of this lab was to become more familiar with programming using Arduino IDE and the Artemis board. 

## Lab 2A: Bluetooth

The purpose of this lab was to test Bluetooth by learning how to use Jupyter lab notebooks in conjunction with the Arduino IDE to communicate with the Artemis board. 

### Prelab

ADD SOMETHING HERE

### Configuration

ADD SOMETHING HERE

### Task 1: ECHO Command

First, I completed the ECHO function in Arduino IDE. This function can be seen below. 
<p align="center">
<img width="550" src="photos/Echo_Arduino.png">
</p>
<br>

I then called the function in Jupyter, using the code shown below. The command sends the robot "HiHello" and the robot returns "Robot says -> HiHello."
<p align="center">
<img width="350" src="photos/Echo_Python_Crop.png">
</p>
<br>


### Task 2: SEND_THREE_FLOATS Command

As with the ECHO command, in order to implement SEND_THREE_FLOATS, I first had to write the necessary code to perform the desired task in Arduino. This code can be seen below.

<p align="center">
<img width="550" src="photos/Send_three_ard_fun.png">
</p>
<br>


I then called SEND_THREE_FLOATS using the Jupyter Notebook code shown in the image below. 

<p align="center">
<img width="550" src="photos/Send_three_python_crop.png">
</p>
<br>

This prompted the Artemis to output the floats to the Serial Monitor.

<p align="center">
<img width="550" src="photos/Send_three_ard_sm.png">
</p>
<br>

### Task 3: GET_TIME_MILLIS

As with the first two tasks, GET_TIME_MILLIS needed to be implemented in Arduino. Unlike before, the GET_TIME_MILLIS did not exist in either Arduino or in Jupyter lab. In order to create the command, it first had to be added to enum CommandTypes in Arduino and cmd_types.py, as seen below. 

<p align="center">
<img width="500" src="photos/Enum_arduino.png"> <img width="300" src="photos/Enum_python.png">
</p>
<br>

After the command names were updated, I was able to create the GET_TIME_MILLIS in Arduino IDE.

<p align="center">
<img width="500" src="photos/Millis_arduino.png"> 
</p>
<br>

Then, the following python code was implemented in Jupyter Notebook to obtain the time in milliseconds from the Arduino in the format of "T: " followed by 6 digits.

<p align="center">
<img width="500" src="photos/Millis_python.png"> 
</p>
<br>

### Task 4: Notification Handler

In order to extract time from the string value sent by the Artemis board, I set up a notification handler. The notification handler, shown below, is able to handle three different cases. These cases include different formatting of the timestamp data as well as the addition of temperature data. These three cases pertain to tasks 5, 6, and 7, respectively. Therefore, the notification manager can be used properly for all three tasks. 

<p align="center">
<img width="500" src="photos/Notif_handler.png"> 
</p>
<br>

### Task 5: Data Transfer via Loop Method

To test the effective data transfer rate of Bluetooth communication between my computer and the Artemis board, 
