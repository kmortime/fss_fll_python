# Lesson #1: Using the simulator and the FSS FLL Spring Training Game, write a Python program to move a block to the M08 Scoring area.

1. In a different browser window or browser tab start the simulator: [Launch FSS FLL Simulator](https://kmortime.github.io/gears_pybricks/public/)
  * Once in the simulator, navigate between programming and running the bot using the menu tabs in the upper left.  Try navigating between the menu options.
  * **Blocks:** Allows for using Scratch like programming blocks to build a Python program.  
  * **Python:** This is the python program run by the simulator.  You can write your Python program right in the browser or you can load it using the "File->Load Python from your computer" menu.  Python programs can also be saved back to your computer using the "File->Save Python to your computer" option.
  * **Simulator:** This is the robot simulation environmnent.  You can simulate your Python program on to scale a virtual robot.
  * **File:** Use this to load or save python programs
  * **Robot:** Use this to select your robot.  For this lesson, the default robot "FSS FLL Single Sensor Line Follower" is selected already.

2. Navigate to the "Simulator" tab.  
  * Click on the "Follow" camera mode in the upper right to toggle the camera style between "Follow," "Top" and "Arc."  You can use the right and middle mouse buttons to move the camera around.
  * Inspect the game board.  The virtual robot is in the lower right start area quarter circle.  There are two movable food cubes on the game table along with an stationary speed bumb north of Mechanic Street. 
  * Your mission is to move one food cube into the M08 Food circle one into the M09 Regolith circle.
   
3. Now it is time to create your python program!
  * Navigate to the "Python" tab.
  * You can see some of the python program is already in the text editor.
  * There are many online resources for learning the Python programming language.  For this first lesson, here are some tips:
    * Comments are preceeded by the symbol "#"
    * For this lesson, you will be adding your changes below the comment "# Here is where your code starts"  Do not modify the code above that comment as it includes many of the features and functions required to make your robot work.
    * The Python programming language like many programming languages is strict when it comes to spelling, punctuation, and indentation.  
  * Use the code in the example below to attempt to move the food cube into the M08 food circle.  Copy it from here and paste it into the Python tabs text editor.
    * Be careful to either replace all the contents in the python tab text editor with what is listed below or only include the code below "# Here is where your code starts"  
    * Try adjusting the robot speed which is the first number "150" in the drive function "robot.drive(150, 0)"
    * Try adjusting the turn angle which is the second number "0" in the drive function
    * Try adjusting how far the robot drives which is the "1400" in the while loop "while motorB.angle() <= 1400:"

4. Go to the "Simulator" tab and click the play button to run the simulation.
    * Here's what you should see happen: [Lesson #1 video](https://www.youtube.com/watch?v=SjU45OVglmE) 



```python
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


################################
# Here is where your code starts
################################

# Attempt to push block into M08 Food Circle
print("Pushing Block into M08 Food Circle...\n")
while motorB.angle() <= 900:
  robot.drive(150, 0)
robot.stop()
print("MotorB has reached angle " + str(motorB.angle()))
motorB.reset_angle(angle=0)
motorC.reset_angle(angle=0)
robot.stop()
left_motor.brake()
right_motor.brake()

# Return to the launch Area
print("Returning to the launch area...\n")
while motorB.angle() >= -900:
  robot.drive(-150, 0)
robot.stop()
left_motor.brake()
right_motor.brake()
print("MotorB has reached angle "+ str(motorB.angle()))
motorB.reset_angle(angle=0)
motorC.reset_angle(angle=0)
```
