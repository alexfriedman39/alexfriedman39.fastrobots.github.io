# Lab 1 

## Lab 1A: The Artemis Board

The purpose of this lab was to become more familiar with programming using Arduino IDE and the Artemis board. 

### Prelab

ADD SOMETHING HERE

### Configuration

ADD SOMETHING HERE

### Task 1: Hook Artemis Board up to Computer

After updating my Arduino and installing the Sparkfun Appollo3 boards manager I was able to select the correct Board. After downloading the CH340 Driver, I was able to select the correct Port. 

<p align="center">
<img width="400" src="photos/Board_port.png">
</p>
<br>

### Task 2: Blink

TAKE VIDEO AND UPLOAD

### Task 3: Serial

TAKE VIDEO AND UPLOAD

### Task 4: Temperature Sensor

TAKE VIDEO AND UPLOAD

### Task 5: Microphone

UPLOAD VIDEO

### Additional Tasks for 5000-Level

[Watch Video](https://youtu.be/bLosVyVbwyg)

## Lab 2A: Bluetooth

The purpose of this lab was to test Bluetooth by learning how to use Jupyter lab notebooks in conjunction with the Arduino IDE to communicate with the Artemis board. 

### Prelab

ADD SOMETHING HERE

### Configuration

ADD SOMETHING HERE

### Task 1: ECHO Command

First, I completed the ECHO command in Arduino IDE. This command can be seen below. 
<p align="center">
<img width="550" src="photos/Echo_Arduino.png">
</p>
<br>

I then called the command in Jupyter, using the code shown below. The command sends the robot "HiHello" and the robot returns "Robot says -> HiHello."
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

As with the first two tasks, GET_TIME_MILLIS needed to be implemented in Arduino. Unlike before, the GET_TIME_MILLIS did not exist in either Arduino or in Jupyter lab. In order to create the command, it first had to be added to enum CommandTypes in Arduino and cmd_types.py, as seen below. I repeated this process when creating commands in Arduino for tasks 4-7.

<p align="center">
<img width="300" src="photos/Enum_arduino.png"> <img width="300" src="photos/Enum_python.png">
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
<img width="500" src="photos/Notif_hand_edited.png"> 
</p>
<br>

### Task 5: Data Transfer via Loop Method

To test the effective data transfer rate of Bluetooth communication between my computer and the Artemis board, I created a loop in Arduino that obtained the time in milliseconds and then sent it to Jupyter Lab on my computer. Once called, the loop ran for 10 seconds. In addition to recording the time, I kept track of how many times the loop ran during the 10 seconds. This "count" was sent to computer along with the time in milliseconds to more easily calculate the average effective data transfer rate over 10 seconds. The code is shown below. 

<p align="center">
<img width="500" src="photos/Time_loop_ard.png"> 
</p>
<br>

The notification handler was responsible for receiving and processing the time sent to the computer by the Artemis board. It separated time and count values, placing time values into an array. In order to better visualize the data being sent from the Artemis board, the notification handler printed both count and time in milliseconds. The beginning and end of a sample output can be seen below. 

<p align="center">
<img width="300" src="photos/Time_loop_sample_beg.png"> <img width="300" src="photos/Time_loop_sample_end.png">
</p>
<br>

Since approximately 380 time values were transferred from the Artemis board to my computer within 10 seconds, the effective data transfer rate using this method is 38 values per second on average.  

Finally, the code that was called in Jupyter notebook to activate the notification handler and the time loop can be seen below. 

<p align="center">
<img width="500" src="photos/Time_loop_py.png"> 
</p>
<br>

### Task 6: Data Transfer via Array Method

As above in task 5, the purpose of this task is to send timestamps from the Artemis board to my computer. However, to improve efficiency, each time stamp was placed into an array in Arduino. This array, along with the temperature array used in task 7, had to be defined globally to allow them to be accessed outside of the loop command. 

EXPLAIN WHY CHOSE 500 AS ARRAY SIZE (GOT _X_ SECONDS, ENOUGH TO DETERMINE EFFICIENCY WITHOUT WASTING TOO MUCH STORAGE)

<p align="center">
<img width="550" src="photos/Global_arr.png">
</p>
<br>

Creating these arrays allowed all the values entire array to be sent back at once, instead of one value at a time. I created a command, TIME_LOOP_ARR, in Arduino to add the timestamp values to the timestamp array. I recorded the time values in microseconds instead of milliseconds because I wanted greater accuracy when calculating the rate of data transfer for this method. 

<p align="center">
<img width="550" src="photos/time_arr_ard.png">
</p>
<br>

Then, I created the SEND_TIME_DATA command which looped over the timestamp array to send the timestamps to the computer. 

<p align="center">
<img width="550" src="photos/send_time_arr.png">
</p>
<br>
The Jupyter notebook code used to call the commands and activate the notification handler can be seen below. The loop command was called first to generate the array, and then the notification handler was activated in order to properly receive and process the timestamps sent to the computer. 

<p align="center">
<img width="550" src="photos/send_arr_py.png">
</p>
<br>

The beginning and end of a sample output can be seen below. This method was much more efficient, as 500 timestamps were recorded in only ____ seconds. Therefore, the effective data transfer rate using this method was found to be ___ values per second on average.

ADD SAMPLE OUTPUT PIC

### Task 7: Adding Temperature Readings

It is possible for the Artemis board to record temperature values with their timestamps. In order to do this, I created the GET_TEMP_READINGS command, seen below, which acted similarly to TIME_LOOP_ARR and SEND_TIME_DATA from task 6, except it also recorded temperature values and placed them in their corresponding array. Once all values were recorded, the temperature values were sent back on the same string as their corresponding timestamp. 

<p align="center">
<img width="400" src="photos/Temp_ar.png">
</p>
<br>

The Jupyter notebook code below initialized time and temperature arrays, initialized the notification handler to receive and process the incoming data, and called the GET_TEMP_READINGS command. 

<p align="center">
<img width="550" src="photos/Temp_py.png">
</p>
<br>

The beginning and end of sample time and temperature values after processing by the notification handler can be seen below. 

<p align="center">
<img width="300" src="photos/Sample_temp_beg.png"> <img width="300" src="photos/Sample_temp_end.png">
</p>
<br>

### Additional Tasks for 5000-Level

#### 1. Effective Data Rate and Overhead

In order to investigate effective data rate and overhead for varying byte sizes, I first made a REPLY command in Arduino. This command would return a char array when prompted by the computer as well as its corresponding timestamp. The length of the char array could be changed between trials in order to vary the byte size since one char character is equivalent to one byte. So, for example, toReply[] shown in the code below is 25 bytes. 

<p align="center">
<img width="550" src="photos/Data_rate_ar.png">
</p>
<br>

I created a loop in Jupyter notebook that would call REPLY 100 times. This made it easier to find the data rate, as I could average over 100 samples. The notification handler was also activated to receive and process the timestamps sent by the Artemis board. The code can be seen below. 

<p align="center">
<img width="550" src="photos/Data_rate_py.png">
</p>
<br>

An example of the timestamps printed for a 25-byte reply can be seen below.

<p align="center">
<img width="550" src="photos/Data_rate_output.png">
</p>
<br>

After testing a variety of size replies, I was able to create the following graph that relates reply size to data rate:

<p align="center">
<img width="550" src="photos/Data_rate_chart.png">
</p>
<br>

As demonstrated by these results, shorter packets do appear to introduce more overhead than larger packages. Howevever, there seems to be a limit to this trend. The data rate increases with reply size up until around 100 bytes. The data rate levels off between 100 and 120, decreasing very slightly. This indicates that although larger replies can be used to reduce overhead, there is a limit to the ideal reply size.

#### 2. Reliability
