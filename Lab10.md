# Lab 10: Grid Localization using Bayes Filter

## Overview

Since robots do not inherently know where they are located, robot localization is commonly used to determine where they are with respect to their environment. Localization works best when probablilistic methods are used in conjunction with sensors.

To the robot, the world is a continuous 3D space with dimensions x, y, and θ. In order to perform calculations, the space is discretized into 1 x 1 ft cells with 20 degrees of orientation. The map is further constrained to [-5.5, 6.5) ft in the x direction, [-4.5, 4.5) ft in the y direction, and [-180, 180) in θ. These constraints are a result of the map, which is the same as Lab 9. Therefore, the total number of cells are 12, 9, and 18, respectively. 

As the robot moves, these cells are updated with the probability, calculated using Bayes filter, that the robot is in the corresponding position and orientation. The probability values across all cells should always sum to 1. By recording which cell has the highest probability after every step, we can follow the robot's progress through the environment with a degree of accuracy. 

## Bayes Filter Implementation

In order for the robot to accurately calculate cell probabilities, the following Bayes filter algorithm needed implementation: 

*** ADD IN BAYES FILTER SCREENSHOT ***

Implementation was done in python by completing the helper functions outlined in the lab 10 notebook. 

### compute_control()

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

### odom_motion_model()

First, this function obtains odometry values by utilizing compute_control(). Then, it calculates their individual probabilities and returns the overall probability that the robot is at this current state given a previous state. The individual probabilities are calculated using a Gaussian distribution centered on the control input with the given standard deviations. 

<div style="height:400px; overflow:auto;">
<pre><code class="language-python"
def odom_motion_model(cur_pose, prev_pose, u):
    """ Odometry Motion Model

    Args:
        cur_pose  ([Pose]): Current Pose
        prev_pose ([Pose]): Previous Pose
        (rot1, trans, rot2) (float, float, float): A tuple with control data in the format 
                                                   format (rot1, trans, rot2) with units (degrees, meters, degrees)


    Returns:
        prob [float]: Probability p(x'|x, u)
    """
    
    delta_rot_1, delta_trans, delta_rot_2 = compute_control(cur_pose, prev_pose)

    prob_rot_1 = loc.gaussian(delta_rot_1, u[0], loc.odom_rot_sigma)
    prob_trans = loc.gaussian(delta_trans, u[1], loc.odom_trans_sigma)
    prob_rot_2 = loc.gaussian(delta_rot_2, u[2], loc.odom_rot_sigma)

    prob = prob_rot_1 * prob_trans * prob_rot_2

    return prob
</code></pre>
</div>

### prediction_step()

This purpose of this function is to calculate the belief probabilities. Essentially, it does this by obtaining the odometry values, looping over all the cells, and implementing the equation in line 3 of the Bayes filter algorithm shown above. Once all beliefs have been calculated, they are normalized so that they sum to 1. 

<div style="height:400px; overflow:auto;">
<pre><code class="language-python"
def prediction_step(cur_odom, prev_odom):
    """ Prediction step of the Bayes Filter.
    Update the probabilities in loc.bel_bar based on loc.bel from the previous time step and the odometry motion model.

    Args:
        cur_odom  ([Pose]): Current Pose
        prev_odom ([Pose]): Previous Pose
    """
    u = compute_control(cur_odom, prev_odom)

    loc.bel_bar = np.zeros((mapper.MAX_CELLS_X, mapper.MAX_CELLS_Y, mapper.MAX_CELLS_A))

    for x_prev in range(mapper.MAX_CELLS_X):
        for y_prev in range(mapper.MAX_CELLS_Y):
            for theta_prev in range(mapper.MAX_CELLS_A):
                if (loc.bel[x_prev, y_prev, theta_prev] < 0.0001): 
                    continue

                for x_cur in range(mapper.MAX_CELLS_X):
                    for y_cur in range(mapper.MAX_CELLS_Y):
                        for theta_cur in range(mapper.MAX_CELLS_A):
                            p = odom_motion_model(mapper.from_map(x_cur, y_cur, theta_cur), mapper.from_map(x_prev, y_prev, theta_prev), u)
                            loc.bel_bar[x_cur, y_cur, theta_cur] += p * loc.bel[x_prev, y_prev, theta_prev]

    loc.bel_bar /= np.sum(loc.bel_bar)
</code></pre>
</div>

### sensor_model()


<div style="height:400px; overflow:auto;">
<pre><code class="language-python"

</code></pre>
</div>

### Acknowledgements 
