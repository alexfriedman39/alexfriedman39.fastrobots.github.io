# Lab 6: Orientation Control 

## Prelab

As with Lab 5, I added a flag, PID_ORI_RUNNING, so that the robot could start and stop as a result of Bluetooth input. I created the cases BEGIN_ORI_PID and STOP_ORI_PID to turn the flag on and off. 

*** ENABLE THE DMP, JUST CAN'T OPEN FILE TO EDIT ATM??***

uncommented line in ICM_20948_C.h to define ICM_20948_USE_DMP

then, opened example Example7_DMP_Quat6_EulerAngles, and added the setup code to my ble setup 

the example also had code that I used as an example to implement DMP functionality in my main loop

