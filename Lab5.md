# Lab 5: Linear PID Control & Linear Interpolation

## Prelab

First, before starting on this lab, I rewired my car to make the electronics more compact. I made sure to keep the connections the same as before, but I cut the wires down significantly and organized them so that the motor drivers, arduino, and battery fit in the back of the car. I placed the TOF sensors on the front and back of the car, and placed the IMU at the front of the car on top. An image of the final version is shown below.

*** FINAL WIRING OF CAR PHOTO *** 

In addition, before I started to implement a PID controller, I had to make sure I had a good debugging system. In order to do this, I first created a new python script to allow the car to be controlled via Bluetooth. I implemented corresponding commands in Arduino, including ...

## Task...

At first, to test P control, I had a hard stop implemented. Using this method I got the robot to hit the wall gently, and reverse to approximately 1 ft from the wall, then stop. 

*** ADD VIDEO IN BELOW *** 

The K value that worked the best was 0.15. The cause of the hard stop was the arduino code shown below

*** ADD IN CODE ***

The distance and speed graphs that resulted from the addition of a hardstop at Kp = 0.15 can be seen below

*** ADD IN GRAPHS ***

As you can see, the robot did not oscillate, and instead stopped not exactly at 1 ft from the wall. 


notes: 

have videos of P controller and varying Kp values

first implement integral and derivative control to get PID
- done, have code in place that calculates speed using integration and derivation

integral windup 
- have video where I showed that it would not slow down as it got near a wall
- made it so that integral value contribution did not exceed a certain speed, especially as we neared the wall
- values too low did not reach the desired foot marker
  - maybe retest this since I have since increased the backwards minimum speed to account for bad wheels 

then need to:
- change code so that new speed is calculated even when distance sensor doesn't update
  - should result in faster frequency
  - compare this to before
  - I just did this, see screenshots for before and after
- record frequency
  - found for K controller, got approx 16.5 samples/second for dist measurements (just gotten from one sample, didn't look at others)
  - refer to stefan's
  - got approx 65 Hz after changing it
- test results from varying distances (2-4 m?)
  - requires you to change to long distance mode to go from these distances
- figure out how to document linear speed???
  - not in the task list tho
  - they wanted THREE repeated videos to demonstrate reliability 
- use extrapolation to calculate derivative control
  - use else if for when distance sensor data is NOT ready
  - see eqn in Mikayla's code
  - do we need to do LPF/derivative kick??
