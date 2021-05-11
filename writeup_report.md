# CarND-Path-Planning-Project

Writeup report for the submission of the Path Planning Project

---

## Introduction

In this project I implemented a simple path planner in C++ using localization data and sensor fusion data provided by a simulator. The generated path is used to navigate the vehicle on a highway in order to avoid obstacles.

## Path Planning

My implementation is based on the approach presented in the project Q&A. It generates 50 waypoints for the vehicle to follow. The waypoints generated are on a generated spline between the vehicle and the point 30 meters ahead of it along the lane. If there is no obstacle within 30 meters the lane is the same as the one the vehicle is in. Obstacle detection code can be found from lines 112 through line 132 in main.cpp.

If there is an obstacle in front of the vehicle within 30 meters, then the following logic decides what to do. First it checks if there is a lane on the left-hand side and whether that lane has any obstacles in it. An obstacle in an adjacent lane can be a vehicle in the sensor fusion data that is either in front of the ego vehicle within 30 meters or a vehicle behind the ego vehicle within the same distance. Obstacle detection in the left-hand side lane can be seen in the code from line 136 through line 166 in main.cpp.

If no such obstacle found then the target lane is changed to this adjacent lane. This can be seen in main.cpp from line 168 through line 172. As a result new waypoints added to the end of the path will lead to that adjacent lane.

If there is no lane on the left-hand side or it is not free from obstacles, then the same checks will be performed for the right-hand side lane. (Lines from 175 through line 212 in main.cpp.)

If the lane change could not be performed, then the vehicle slows down gradually in order not to collide with the obstacle in front of it. This can be seen in main.cpp from line 217 through line 220.

If there is no obstacle in the lane the vehicle is in, the vehicle speeds up to 49 mph, just below the speed limit. The speed up is performed gradually in order not to exceed the limits for acceleration and jerk. This can be seen in main.cpp from line 221 through line 224.

The resulting behaviour of the vehicle in the simulator can be seen in the following video:
https://youtu.be/_sxbg4Pm6iA

## Reflection

Although the vehicle can run for a very long distance without collision, there is a chance for incidents to happen. For example, the deceleration of the obstacle in front of the ego vehicle can be faster than the fixed deceleration of the ego vehicle. The latter can be a parameter or can be adjusted dynamically based on the obstacle's data.

Another weakness of my current path generation can be that it very likely could not avoid a collision during lane change if another vehicle was changing to the same lane from the other side of that lane at the same time.

Currently the vehicle cannot decide on a lane change based on checking the second adjacent lane - changing two lanes to avoid an obstacle.

A more elegant solution for path planning would involve multiple cost functions choosing from many possible generated trajectories.
