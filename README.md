# Path Planning
Udacity Self-Driving Car Engineer Nanodegree Program
   
This project involves using a Simulator developed by Udacity which can be downloaded [here](https://github.com/udacity/self-driving-car-sim/releases/tag/T3_v1.2).

To run the simulator on Mac/Linux, first make the binary file executable with the following command:
```shell
sudo chmod u+x {simulator_file_name}
```
## Dependencies

* **g++ 9.3** Installation Instructions: [Mac](https://developer.apple.com/xcode/features/) [Windows](http://www.mingw.org/) 
* **Ubuntu Terminal** for running UNIX Terminal on Windows. [download](https://aka.ms/wslubuntu2004)
* **uWebSocketIO** for smooth data flow between simulator and code. [download](https://github.com/uWebSockets/uWebSockets)  
    ```
    <Navigate to the project folder using Ubuntu terminal>
    chmod u+x install-ubuntu.sh
    ./install-ubuntu.sh
    ./install-linux.sh
    ```
    If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets 
    cd uWebSockets
    git checkout e94b6e1
    ```
* **cmake 3.5** [Installation Instructions](https://cmake.org/install/)  
* **make 4.1 (Linux and Mac), 3.81 (Windows)**  Installation Instructions : [Mac](https://developer.apple.com/xcode/features/) [Windows](http://gnuwin32.sourceforge.net/packages/make.html)

## Installation and Usage

This repository includes two files that can be used to set up and install uWebSocketIO for either Linux or Mac systems. For windows you can use either Docker, VMware, or even Windows 10 Bash on Ubuntu to install uWebSocketIO.

Once the install for uWebSocketIO is complete, the main program can be built and ran by doing the following from the project top directory.

    mkdir build && cd build
    cmake .. && make
    ./path-planning

Alternatively some scripts have been included to streamline this process, these can be leveraged by executing the following in the top directory of the project:

    ./clean.sh
    ./build.sh
    ./run.sh
    
On the Ubuntu Terminal, we can observe that the program is listening to a port. Now, if the Simulator is launched, it should automatically connect and execute.

## Data

### The map of the highway is in data/highway_map.txt
Each waypoint in the list contains  [x,y,s,dx,dy] values. x and y are the waypoint's map coordinate position, the s value is the distance along the road to get to that waypoint in meters, the dx and dy values define the unit normal vector pointing outward of the highway loop.

The highway's waypoints loop around so the frenet s value, distance along the road, goes from 0 to 6945.554.

Here is the data provided from the Simulator to the C++ Program

### Main car's localization Data (No Noise)

["x"] The car's x position in map coordinates

["y"] The car's y position in map coordinates

["s"] The car's s position in frenet coordinates

["d"] The car's d position in frenet coordinates

["yaw"] The car's yaw angle in the map

["speed"] The car's speed in MPH

### Previous path data given to the Planner

//Note: Return the previous list but with processed points removed, can be a nice tool to show how far along
the path has processed since last time. 

["previous_path_x"] The previous list of x points previously given to the simulator

["previous_path_y"] The previous list of y points previously given to the simulator

### Previous path's end s and d values 

["end_path_s"] The previous list's last point's frenet s value

["end_path_d"] The previous list's last point's frenet d value

### Sensor Fusion Data, a list of all other car's attributes on the same side of the road. (No Noise)

["sensor_fusion"] A 2d vector of cars and then that car's [car's unique ID, car's x position in map coordinates, car's y position in map coordinates, car's x velocity in m/s, car's y velocity in m/s, car's s position in frenet coordinates, car's d position in frenet coordinates. 

## Details

1. The car uses a perfect controller and will visit every (x,y) point it recieves in the list every .02 seconds. The units for the (x,y) points are in meters and the spacing of the points determines the speed of the car. The vector going from a point to the next point in the list dictates the angle of the car. Acceleration both in the tangential and normal directions is measured along with the jerk, the rate of change of total Acceleration.

2. There will be some latency between the simulator running and the path planner returning a path, maybe just 1-3 time steps. During this delay the simulator will continue using points that it was last given, because of this its a good idea to store the last points we have used so we can have a smooth transition. previous_path_x, and previous_path_y are helpful for this transition since they show the last points given to the simulator controller with the processed points already removed.

## Editor Settings

It is recommended to use the following settings:

* indent using spaces
* set tab width to 2 spaces (keeps the matrices in source code aligned)

## Looking forward

There are some things that can be used to make this project much more refined. I list some possible avenues for experimentation below:

### Using JMT

I found jerk minimising trajectories to be slightly complicated. However, it might be a better way to compute the best trajectory than splines, subject to experimentation.

### Using a better decision framework

I found the way we select the lane to shift to, very static. Upon finding a vehicle ahead of us, we first check left, and if not possible, then check right. A better approach would be to consider both lanes, left and right, and to select the lane where the next car ahead is farthest, so that we can possibly overtake the car ahead of us. Instead, using this static approach makes our vehicle susceptible to get stuck in local optima.

### Using cost functions

A better way is to design cost functions in a manner that the decision making is more dynamic. Like the weights in a neural network, the weights for costs, perhaps, can be generated using offline reinforcement learning in a simulated environment such as this. After several iterations, perhaps the car can be made to learn its own costs, which can be used in an online system. Even a more static cost function approach would work better than the current conditional logic approach that has been used.

## Output

A video of the output titled **Output.mp4** can be found in the repository.
