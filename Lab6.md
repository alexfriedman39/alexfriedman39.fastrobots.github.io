# Lab 6: Orientation Control 

## Prelab

As with Lab 5, I added a flag, PID_ORI_RUNNING, so that the robot could start and stop as a result of Bluetooth input. I created the cases BEGIN_ORI_PID and STOP_ORI_PID to turn the flag on and off. In addition, I continued to use the EDIT_GAINS case that I elaborated on in Lab 5, which made it much easier for me to test different gain values as I tested P and PI control.

## DMP SETUP 
In order to mimize yaw drift, I decided to use my IMU's onboard DMP. Following the instructions given in the "Digital Motion Processing for Orientation" handout, I first uncommented line 29 in CM_20948_C.h to define ICM_20948_USE_DMP. Then, I opened the example file Example7_DMP_Quat6_EulerAngles, and added the code shown below to my setup() function. 

*** DMP SETUP CODE ***

I also added code from the example's loop() function to mine. This allowed me to record the orientation quaternions from the DMP while the robot was running. To convert the quaternions to the yaw angle, I created the function yaw_from_quat, which is seen below. I referred to the example Wikipedia code linked on the handout to create this function.

*** YAW_FROM_QUAT FUNCTION***

## PID Control

I implemented PID control similar to Lab 5, except using yaw angle instead of distance from the wall. I created the function shown below to calculate PWM speed based on the error in yaw angle. 

*** Speed_calc_ori ***

If the resulting PWM speed calculation was positive, the robot would respond by turning left. When the value was negative, the robot would turn right. This can be seen in the code snippet from my loop() function shown below. This code also shows how I recorded debugging data. I created a new case, SEND_SPEED_ANGLE, that would loop through the debugging data and send it from the robot to my computer after running each trial. 

*** loop code snippet ***

### P Control
At first, I tested proportional control

### PI Control

need to change code so that windup isn't quite as prevalent when Ki is increased 

decided not to use derivative control because the overshoot wasn't a problem (very small)

see screenshots of plots to calculate frequency, can compare to distance sensor frequency
approx 14 samples/second

5000 level - wind up implementation and discussion 

