# Self-Driving Car Engineer Nanodegree

## Term 3 Project 1: Path Planning

The goal for this project was to implement a path planner driving along a highway in the SDC simulator. During the drive the velocity should be pretty close to the speed limit but not exceed it. Other cars generated randomly by the generator need to be passed to avoid collisions.
The project specification was given [here](https://review.udacity.com/#!/rubrics/1020/view).

### Approach

The courses previous to this project brought a lot of information for search, prediction, behavior planning and trajectory generation. This was quite interesting but not that helpful for this project. The most helpful information was given directly in the preparation chapters of this project, especially in the video of David and Aaron. My code is based on the example given and discussed by the two. The missing part was to implement a decision tree whether to brake or to change lanes.

1.  When to brake?

Due to my driving experience on the Autobahn I pretty much don't like to brake. So my car is accelerating all the time it is below 49.5 mph to hold the velocity close to the speed limit and does only use the brake when entering a dangerous situation.
In this case this situation is, if a car is directly in front of it. The value of the deceleration is based on the distance to the car in front.
```
// brake hard or soft dependend on the distance to the car in front
else if (emergency_brake){
    ref_vel -= 0.224;
}
else {
    ref_vel -= 0.112;
}
```

2. When to change lanes?

If there is a car detected in front of the own car in the sensor fusion data the possibilities to switch lanes will be checked. Therefore the indices in the sensor fusion data for left, middle and right lane are saved and taken to account in the following steps. For the left and the right lane the sensor fusion data of the middle lane will be checked. For the middle lane the other lanes are prioritized to first check the left lane and change if no obstacle. If there is an other car the right lane is checked. The area to be scanned for other vehicles is shown in the following picture. There are no cars allowed in the red area and additionally in the blue area there are no cars allowed with significantly lower velocity.
![area to be scanned](/images/scan_neighbour_lane.png)

3. Combine lane changes and brakes

The car always tries to go full speed. Therefore it will change the lane if there is a car in front of it. Before every of this changes the destination lane is checked for other cars. Only if it is safe the car will chang the lane. In any other case the car will brake and brake even harder if there is an obstacle with close distance.

### Challenges

I was struggeling in situations when the car needs to brake. It was getting too slow and needed some time to accelerate again. Therefor I implemented the two stages of brake (normal and emergency) and addtionally set the horizon of the scanned areas in the sensor fusion data dependant to the current speed of the vehicle. So when slowing down it does not have to wait for all obstacles leaving the scanned area and can accelerate earlier.

### Conclusion

First I thought the project would be really hard, but due to the preparation chapters the complex part was taken care of and I could focus on the decision tree. In the end I kept my car running for nearly 20 minutes around the course without any crash.
![sucessful drive](/images/path_planning_success.png)




