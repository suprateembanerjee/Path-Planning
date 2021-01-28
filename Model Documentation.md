# CarND-Path-Planning-Project
Self-Driving Car Engineer Nanodegree Program
   

## Reflections

The path planner used in this project uses Frenet Coordinates on a relatively sparse Highway environment to generate and select trajectories that might be optimal for the autonomous vehicle to traverse.

There are a few parts to this project:
* Generating trajectories
* Determining feasibility
* Selecting the optimal trajectory

### Generating Trajectories

This phase involved generating trajectories for the car. We had to check whether the car already had some previous path, and in either scenario, generate points that would dictate how the car should move in the environment. I extrapolated the car's orientation of movement using arctangent estimation if we had a previous path, or extrapolate the car's previous possible position using the car's yaw in case of no prior path.

I then generated three points ahead of the car, in Frenet, at 30m, 60m and 90m distance, and converted them to X-Y coordinates so that I could use them in the map. Then, I shifted the car's orienation to 0 degrees, so as to easily calculate relative positions of other vehicles around the car.

These trajectories however, were not jerk minimized, and would not meet jerk constraints. I used spline to enable smooth motion, as opposed to JMT, which while being covered in the classroom, I found a little convoluted to actually apply in this scenario. For every change in direction, the spline extrapolated points that made the transition smoother.

Finally, I rotated my orientation back to match the map so that there is no coordinate mismatch, before pushing the points to next_x_vals and next_y_vals, which were used to capture the geenrated trajectories.

### Determmining feasibility

I used several flags, namely car_ahead, car_left, and car_right to indicate whether a left or right lane shift is feasible, when a car is detected ahead of us.

### Selecting an optimal trajectory

Here we could use a couple of techniques. In more complex environments, an undoubtedly better way would be to use cost functions. However, given the sparseness of the highway environment, I found it fared well using simple conditional logic to choose the optimal behavior. 

## Looking forward

This project was reasonably complex, and given a time crunch (being a couple of weeks behind schedule), I did the minimum that the project needed to pass the rubric. However, there are multiple things that can be used to make this project much more refined. I list some possible avenues for experimentation below:

### Using JMT

I found jerk minimising trajectories to be slightly complicated. However, it might be a better way to compute the best trajectory than splines, subject to experimentation.

### Using a better decision framework

I found the way we select the lane to shift to, very static. Upon finding a vehicle ahead of us, we first check left, and if not possible, then check right. A better approach would be to consider both lanes, left and right, and to select the lane where the next car ahead is farthest, so that we can possibly overtake the car ahead of us. Instead, using this static approach makes our vehicle susceptible to get stuck in local optima.

### Using cost functions

A better way is to design cost functions in a manner that the decision making is more dynamic. Like the weights in a neural network, the weights for costs, perhaps, can be generated using offline reinforcement learning in a simulated environment such as this. After several iterations, perhaps the car can be made to learn its own costs, which can be used in an online system. Even a more static cost function approach would work better than the current conditional logic approach that has been used.

## Some issues that need fixing

The only real issue with this project seems to be apparent confusion in decision making. When shifting lanes, sometimes the car detects another obstacle down the lane it is shifting to, and then gets confused whether to keep shifting lanes or to stay in the previous lane. Problem is, now it already has shifted from its previous lane, and so it keeps driving along a lane line, outisde any particular lane. This issue is not observed while shifting back to center lane after an over take. However, on average, this issue mostly doesn't occur, and 10 Mile runs are fairly common while testing the system.
