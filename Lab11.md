# Lab 11: Localization on the Real Robot

## Simulation Test

Before testing on my robot, I ran localization in simulation using the lab11_sim.ipynb notebook. The results of this test can be seen in the image below. As in Lab 10, the probabilistic belief, plotted in blue, was much closer to ground truth, green, than odometry, red. 

*** INSERT SIMULATION PICTURE ***

The Localization class was therefore verified since the probabilistic belief, calculated using Bayes, behaved accurately with respect to ground truth.

## Implementating Localization on the Robot

Once I determined that Localization functioned correctly, I implemented perform_observation_loop() in the RealRobot class found in the lab11_real.ipynb notebook. This function was implemented using my START_MAP and SEND_MAP functions from Lab 9. START_MAP causes the robot to rotate 360 degrees in place while collecting distance data from the TOF sensor at specified increments. In order to perform the localization, I had to change the yaw angle increment from 10 to 20 degrees. This decreased the number of data points collected per run from 36 to 18. After the data is collected, SEND_MAP is used to send the time, distance, and yaw data from the robot so it can be processed by the notification handler and analyzed. 

*** ADD IN CODE ***

Note that I used asyncio.sleep() to ensure the python code would not continue until the robot executed its commands. This ensured that all data was collected and sent back without blocking or being blocked by the execution of perform_observation_loop(). In addition, after the distance and yaw data were received, I made sure to convert the lists to a numpy column array so they could be properly utilized in the localization code. Before this conversion, each data point was converted into a float in the notification handler, which is shown below, so that calculations could be done properly.

*** NOTIFICATION HANDLER ***

In the video below, the modified START_MAP command is executed on my robot. It completes a full 360 degree turn, stopping at approximately 20 degree intervals to record a total of 18 distance measurements. 

*** VIDEO OF ROBOT TURNING ***


## Results

After verifying that the START_MAP and SEND_MAP commands worked, I began testing localization with my robot. I tested at all four locations, and the results are shown below. Before each test, I made sure the robot was centered on each location and the TOF sensor started facing right in the map, as this corresponded to 0 degrees. 

### Marked Pose 1: (-3 ft, -2 ft, 0 deg) or (-.9144 m, -.6096 m, 0 deg)

Localization was the most accurate at the first marked pose. During both my tests, I got nearly perfect results. For both, the belief was (-0.914, -0.610, 10) with probabilities of 0.9999999 and 1.0. One of the resulting maps is shown below. The ground truth location is also being plotted, but is not visible since it overlaps with the belief. 

*** loc_1_1 image ***

### Marked Pose 2: (0 ft, 3 ft, 0 deg) or (0 m, 0.9144 m, 0 deg)

Localization was also quite accurate at this marked pose. On my first trial, the belief obtained was (1.219, 0.610, 170) with a probability of approximately 0.998. This result is shown as the blue dot in the map below. Ground truth is plotted in green. Although not entirely accurate, this localization was able to properly place the robot in the upper half of the map. It just seems to have confused the top wall for the wall of the obstruction in the middle of the map which likely occurred because the TOF sensor is at the front of the robot, so distance measurements may have been smaller than expected during the run. 

*** loc_2_1 ***

However, I saw a great improvement in localization on my second trial. This time, the belief was nearly perfect, as it was (0, 0.914, -10) with a probability of 0.9999999. This is evident in the map shown below, as ground truth is not visible since it overlaps with the belief. 

*** loc_2_perfect ***

### Marked Pose 3: (5 ft, -3 ft, 0 deg) or (1.524 m, -0.9144 m, 0 deg)




*** ADD CODE AT END FOR REFERENCE?? ***
