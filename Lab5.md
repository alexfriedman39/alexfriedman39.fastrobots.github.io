# Lab 5: Linear PID Control & Linear Interpolation

## Prelab

First, before starting on this lab, I rewired my car to make the electronics more compact. I made sure to keep the connections the same as before, but I cut the wires down significantly and organized them so that the motor drivers, arduino, and battery fit in the back of the car. I placed the TOF sensors on the front and back of the car, and placed the IMU at the front of the car on top. An image of the final version is shown below.

*** FINAL WIRING OF CAR PHOTO *** 

In addition, before I started to implement a PID controller, I had to make sure I had a good debugging system. In order to do this, I first created a new python script to allow the car to be controlled via Bluetooth. In python, I also edited my notification handler to receive the time, distance, and speed (PWM value) debugging data that would be collected from the robot. The data arrays were constrained to a length of 500 in Arduino to make sure the Artemis's internal RAM storage would not be surpassed.

To make my life easier when testing the robot, I implemented certain commands in Arduino, including EDIT_GAINS and CLEAR_ARRAYS. Both are shown below. The former allowed me to easily change the values of Kp, Ki, and Kd using a Bluetooth command. This made testing much easier, as I did not have to reupload new code each time I wanted to try new gain values. The latter cleared the values stored in the time, distance, and speed arrays. This was useful for tests when the robot did not run for long enough to record 500 values, as it prevented values from previous tests from being sent back to python for analysis. 

*** EDIT_GAINS AND CLEAR_ARRAYS CODE ***

To have the robot start and stop as a result of Bluetooth input, I added a flag, PID_RUNNING. When PID_RUNNING is true, the code will execute, and the robot will begin to move using the PID controller. But, when the flag is false, the robot will be stationary. I implemented two commands in Arduino, BEGIN_PID and STOP_PID, shown below, which turn the flag on and off as a result of Bluetooth input. This made it very easy to control different test cases. 

*** BEGIN_PID AND STOP_PID CODE ***

## P Controller 

Initially, I just tested proportional control to make sure that my code functioned properly. 

*** ADD IN P CONTROL SPEED CALC??***

*** ALSO NEED FREQUENCY ??? ****

For simplicity, I also had a hard stop implemented, which had the robot stop if the calculated PWM value was between -10 and 10, as seen in the code below. This hard stop operated under the assumption that the robot should be close to the desired distane of 1 foot from the wall if when a PWM value this low is calculated.

*** HARDSTOP CODE ***

Using this method, I was able to obtain the result shown below, where the robot hit the wall gently, reversed to approximately 1 foot, and then stopped. The Kp value used in this test was 0.15.

*** ADD VIDEO OF HARDSTOP P CONTROL *** 

The distance and speed graphs that resulted from this P control trial, where Kp = 0.15 can be seen below.

*** ADD IN GRAPHS OF HARDSTOP P CONTROL ***

As you can see from the graphs above, the robot stopped at about 1 foot from the wall. However, we ideally want to see the robot stop at exactly 1 foot, or oscillate about that point. In order to get these oscillations, I commented out the code shown above that caused the hard stop. Instead, when the calculated speed (PWM values) got this low, they were adjusted so that they would be higher than the deadband PWM values found in Lab 4. The full code used to move the robot forward based on the calculated speed is shown below, which includes the deadband adjustment. 

*** ADD IN DEADBAND ADJUSTMENT CODE ***

Using this code, I again tested just a P controller at varying Kp values. I first tested Kp = 0.15 to see how the robot behavior would change with no hard stop. The video and corresponding speed and distance graphs are shown below. This value was too high, as the robot initially collided with the wall. However, it was able to reach a distance from the wall of 1 foot. Since there was no hard stop, it oscillated about this distance. 

*** ADD IN VIDEO FOR KP = .15 AND CORRESPONDING GRAPHS ***

I next tested Kp = 0.10, which did a better job than 0.15, as it prevented the robot from hitting the wall. It also allowed the robot to return back to the desired distance of 1 foot. However, the overshoot that resulted from this gain was still quite large. The corresponding videos and graphs can be seen below. 

*** ADD IN VIDEO FOR KP = .1 AND CORRESPONDING GRAPHS ***

In order to reduce overshoot, I again reduced the Kp value slightly, to 0.07. The resulting video and graphs are shown below. Reducing the gain was able to reduce overshoot, but the tradeoff was that robot speed as it advances towards the wall decreased. But, it was able to reach the desired distance just as effectively as the previous gains tested. As a result, when advancing to testing a PI controller, I kept Kp between 0.7-0.8 to decrease speed and keep the overshoot small.

*** ADD IN VIDEO FOR KP = 0.07 AND CORRESPONDING GRAPHS ***

## PID Control 

Once I obtained ideal proportional gain behavior, I changed my code to incorporate both integral and derivative control when calculating the speed of my car. The corresponding Arduino code can be seen below. 

*** ADD PID SPEED CALC (NO WINDUP PROTECTION) CODE ***

I quickly realized I needed windup protection, as even at extremely low Ki values, the robot would not slow down as it neared an object. My test for this can be seen in the video below. To prevent damage to my TOF sensor, I held onto the car as it neared the wall, since it would have hit the wall at a high speed if released.

*** ADD VIDEO DEMONSTRATING WHY WE NEED WINDUP PROTECTION ***

In order to implement wind-up protection, I added the following constraints, seen in the image below, into my speed (PWM value) calculation. Initally, I constrained the integrator term that contributes to the PWM value to positive or negative 100. However, once the robot moved halfway to the wall, I reduced this constraint to positive or negative 20. Otherwise, the robot would not slow down quickly enough to prevent it from running into the wall. 

*** WINDUP PROTECTION ARDUINO CODE ***

In addition, we were tasked with 

After testing a variety of Ki values, I found that 

overshoot seen at this gain was 

for the videos/graphs i have for Kp = 0.008 and Ki = 0.000001, note that car continued to 1 ft but only 500 data points were collected
->after, changed it to record 1000 since now a higher frequency requires more storage in order to see full car behavior

### 5000 Level - Wind-Up Protection
point to my implementation above
need to add discussion of why it is necessary, though, here

### Acknowledgements
mikayla - wind up protection
stefan - PID implementation

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
