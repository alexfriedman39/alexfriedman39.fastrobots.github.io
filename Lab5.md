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


