# FSS FLL Lesson 3: Line Following

This program follows the line and pushes the food block into the M09 scoring circle.

```code python

#!/usr/bin/env pybricks-micropython

######################################################
# Basic setup for FSS FLL Spring Training Robot
# !!! -- DO NOT CHANGE THIS PART OF THE PROGRAM -- !!!
######################################################

# Import the necessary libraries
import math
import time
from pybricks.ev3devices import *
from pybricks.parameters import *
from pybricks.robotics import *
from pybricks.tools import wait
from pybricks.hubs import EV3Brick

ev3 = EV3Brick()
motorB = Motor(Port.B)
motorC = Motor(Port.C)
left_motor = motorB
right_motor = motorC
robot = DriveBase(left_motor, right_motor, wheel_diameter=56, axle_track=108)
robot.settings(straight_speed=200, straight_acceleration=100, turn_rate=100)

color_sensor_in1 = ColorSensor(Port.S1)

###################################
# Here is where your code starts
###################################

# Proportional Gain for line following
pd = -0.8

# Move away from the wall without hitting it.
print("------------------------------------------")
print("Move away from the wall...")
while motorB.angle() <= 210:
  robot.drive(80, -40)
robot.stop()
left_motor.brake()
right_motor.brake()
print("MotorB has reached angle " + str(motorB.angle()))
motorB.reset_angle(angle=0)
motorC.reset_angle(angle=0)
print("------------------------------------------\n")


# Drive straight until the line following sensor is above the line
print("------------------------------------------")
print("Drive straight to line...")
while motorB.angle() <= 280:
  robot.drive(100, 0)
robot.stop()
left_motor.brake()
right_motor.brake()
print("MotorB has reached angle " + str(motorB.angle()))
motorB.reset_angle(angle=0)
motorC.reset_angle(angle=0)
print("------------------------------------------\n")

# Follow the line to the end.
print("------------------------------------------")
line_follow_rotations = 3.8
print("Follow the line for " + str(line_follow_rotations) + " rotations...")

while motorB.angle() <= line_follow_rotations * 360:
  turn_direction = pd * (color_sensor_in1.reflection() - 50)
  robot.drive(50, turn_direction)
robot.stop()
left_motor.brake()
right_motor.brake()
print("MotorB has reached rotations of " + str(motorB.angle()/360))
motorB.reset_angle(angle=0)
motorC.reset_angle(angle=0)
print("------------------------------------------\n")

# Turn towards the wall
print("------------------------------------------")
print("Tank Turn towards the wall...")
robot.turn(-48)
robot.stop()
left_motor.brake()
right_motor.brake()
print("MotorB has reached angle " + str(motorB.angle()))
motorB.reset_angle(angle=0)
motorC.reset_angle(angle=0)
print("------------------------------------------\n")

# Drive into wall and square up
print("------------------------------------------")
print("Drive into wall and square up...")
while motorB.angle() <= 500:
  robot.drive(50, 0)
robot.stop()
left_motor.brake()
right_motor.brake()
print("MotorB has reached angle " + str(motorB.angle()))
motorB.reset_angle(angle=0)
motorC.reset_angle(angle=0)
print("------------------------------------------\n")


# Back away from wall slightly so the robot can turn
print("------------------------------------------")
print("Back away from wall slightly so the robot can turn...")
while motorB.angle() >= -50:
  robot.drive(-80, 0)
robot.stop()
left_motor.brake()
right_motor.brake()
print("MotorB has reached angle " + str(motorB.angle()))
motorB.reset_angle(angle=0)
motorC.reset_angle(angle=0)
print("------------------------------------------\n")

# Turn 90 degrees to face towards north wall
print("------------------------------------------")
print("Tank Turn 90 degrees to face towards the north wall...")
robot.turn(90)
robot.stop()
left_motor.brake()
right_motor.brake()
print("MotorB has reached angle " + str(motorB.angle()))
motorB.reset_angle(angle=0)
motorC.reset_angle(angle=0)
print("------------------------------------------\n")

# Push block into M09 scoring circle
print("------------------------------------------")
print("Push block into M09 scoring circle...")
while motorB.angle() <= 200:
  robot.drive(100, 0)
robot.stop()
left_motor.brake()
right_motor.brake()
print("MotorB has reached angle " + str(motorB.angle()))
motorB.reset_angle(angle=0)
motorC.reset_angle(angle=0)
print("------------------------------------------\n")

```
