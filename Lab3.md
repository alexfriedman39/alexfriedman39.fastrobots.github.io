# Lab 3

## Prelab

Throughout this lab, we worked with Time-of-Flight (ToF) distance sensors. According to the corresponding datasheet, the default I2C address for a ToF sensor is 0x52. Since we will be using two sensors during the lab, one of the addresses will need to be changed in order to get distance readings from both at the same time. This will be done by wiring pin 8 on the Artemis board to the XSHUT pin on one sensor. This configuration is shown on the wiring diagram below. In addition to the XSHUT pin, the wiring diagram shows how the other ToF pins will be connected to the QWIIC connector.

***ATTACH WIRING DIAGRAM****

Since the ToF sensors will be essential for regulating the robot's movement, they should be attached to the longer QWIIC connectors. This increases the flexibility of their positioning, allowing them to be placed on the front, sides, or back as necessary. For example, it would be beneficial to have one sensor on the front and one on the right side of the car if I am navigating following the wall to my right. But, if I just want to prevent crashes it might be better to have one on the back instead of the front of the car. 

## Task 1: Connect Components

During lab, the ToF sensors were soldered to the longer QWIIC connectors in accordance with the wiring diagram shown above. This is shown in the images below. 

***ATTACH IMAGES OF QWIIC CONNETORS AND TOF***

Next, the QWIIC connectors were connected to the QWIIC breakout board. The IMU was also attached to the breakout board via a shorter QWIIC connector. Then, the QWIIC breakout board was connected to the Artemis board. In addition, the battery was soldered to JST jumper wires in order for it to connect to the Artemis board. The full configuration, minus the battery, is shown below. 

***ADD PIC OF FULL CONFIG***

## Task 2: Scan for I2C Device

Running Example1_wire_I2C gave 0x29 as the I2C address for the ToF sensor. This output is shown below. 

***ADD PIC OF I2C ADDY***

As mentioned in the prelab, the expected I2C address is 0x52. This occurs because the binary form of 0x52 gets shifted 1 place to the right, resulting in 0x29. 

## Task 3: Choose ToF Mode

The short mode is likely ideal for the final robot. The car is quite small, so a maximum distance of 1.3 meters should suffice in preventing the robot from going too far off course. In addition, the short mode is least sensitive to ambient light, which is likely to be present in most settings where the robot is used. The maximum distance measured by long mode actually decreases to less than the short mode's maximum when it's placed in ambient light. As a resut, accuracy will be higher in the short mode, making it the better choice. 

## Task 4: Test ToF Short Mode

In order to test the short mode of my ToF sensor, I taped the sensor to the back of my laptop and lined it up parallel to a wall. The setup is shown in the images below. 

***ADD IN PICS OF SETUP***

I took measurements at distances ranging from 5 to 130 cm. At each distance measured, I collected 5 data points from the sensor in order to obtain repeatability data. The mean of the measured data compared to the actual distance is plotted below. 

***ADD IN MEASURED V ACTUAL PLOT***

***ADD IN CODE***

As seen above, there was a linear relationship between the actual and measured distances over the whole range of distances. To examine the accuracy results more closely, I also plotted the difference between the measured and actual distances over the distance range. 

***ADD IN ACCURACY PLOT***

As shown in the plot above, the measured distance values were pretty accurate throughout the range, deviating about 1.5 cm at most. It does seem that accuracy decreases slightly as distance increases, but the difference is still quite small compared to the order of magnitude of the measured value. For example, at a distance of 100 cm, there is an approximately 1.5 cm deviation. However, this is only a 1.5% error, demonstrating that the distance values are quite accurate. 

The standard deviation was calculated at each distance value to determine repeatability. The plot of standard deviation over distance is shown below. 

***STANDARD DEV PLOT***

As shown in the plot, the standard deviation between data points is very small. This shows that the ToF distance values should be repeatable. However, it is important to note that these values were taken while the sensors were stationary, which will not be the case once they are affixed to the robot. 

Finally, I calculated the ranging time at each distance, which is the time between each data point collected by the ToF sensor. The code I used to calculate it in Python is displayed below. 

***RANGING TIME CODE***

The ranging time remained relatively the same, even as distance increased. It ranged between 0.0505 and 0.05275 seconds. 

## Task 5: Sensors in Parallel

As discussed in the prelab, using both sensors in parallel requires changing the address of one of the sensors. In addition to doing this manually with wiring, the new address needs to be specified in the Arduino code. The code I used to do this is shown below.

***PIC OF ARD CODE W/ NEW DEFINITIONS FOR ADDY***

The setup had to be changed as well. 
