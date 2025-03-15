# Lab 5: Linear PID Control & Linear Interpolation

## Prelab

To make sure I had a good debugging system, 


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

then need to:
- test results from varying distances (2-4 m?)
- figure out how to document linear speed
  - not in the task list tho
- record frequency
  - refer to stefan's
- change code so that new speed is calculated even when distance sensor doesn't update
  - should result in faster frequency
