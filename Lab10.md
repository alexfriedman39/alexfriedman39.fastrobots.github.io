# Lab 10: Grid Localization using Bayes Filter

## Overview

Since robots do not inherently know where they are located, robot localization is commonly used to determine where they are with respect to their environment. Localization works best when probablilistic methods are used in conjunction with sensors.

To the robot, the world is a continuous 3D space with dimensions x, y, and θ. In order to perform calculations, the space is discretized into 1 x 1 ft cells with 20 degrees of orientation. The map is further constrained to [-5.5, 6.5) ft in the x direction, [-4.5, 4.5) ft in the y direction, and [-180, 180) in θ. These constraints are a result of the map, which is the same as Lab 9. Therefore, the total number of cells are 12, 9, and 18, respectively. 

As the robot moves, these cells are updated with the probability, calculated using Bayes filter, that the robot is in the corresponding position and orientation. The probability values across all cells should always sum to 1. By recording which cell has the highest probability after every step, we can follow the robot's progress through the environment with a degree of accuracy. 

## Bayes Filter Implementation

In order for the robot to accurately calculate cell probabilities, the following Bayes filter algorithm needed implementation: 

*** ADD IN BAYES FILTER SCREENSHOT ***

Implementation was done in python by completing the helper functions outlined in the lab 10 notebook. 

### compute_control 

This function takes inputs of the previous and current robot positions (x, y, θ). From these, it calculates the initial rotation, translation, and final rotation in order to model the robot's movement.

<div style="height:400px; overflow:auto;">
<pre><code class="language-python"
def compute_control(cur_pose, prev_pose):
    """ Given the current and previous odometry poses, this function extracts
    the control information based on the odometry motion model.

    Args:
        cur_pose  ([Pose]): Current Pose
        prev_pose ([Pose]): Previous Pose 

    Returns:
        [delta_rot_1]: Rotation 1  (degrees)
        [delta_trans]: Translation (meters)
        [delta_rot_2]: Rotation 2  (degrees)
    """

    x_cur, y_cur, theta_cur = cur_pose
    x_prev, y_prev, theta_prev = prev_pose
    
    delta_rot_1 = mapper.normalize_angle(np.degrees(np.arctan2(y_cur - y_prev, x_cur - x_prev)) - theta_prev,)
    delta_trans = np.hypot(y_cur - y_prev, x_cur - x_prev)
    delta_rot_2 = mapper.normalize_angle(theta_cur - theta_prev - delta_rot_1)

    return delta_rot_1, delta_trans, delta_rot_2
</code></pre>
</div>
