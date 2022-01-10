# Calibration Routine For FSS FLL Spring Training Robot

<p align="center">
<iframe width="560" height="315" src="https://www.youtube.com/watch?v=BGYnlN3-ZhU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</p>

Calibrating the color sensor is an part important of line following.  A line following routine will typically attempt to turn the robot to keep 
the color sensor directly above the edge of parallel black and white lines.  The robot does this by calculating the color sensor's reflected light intensity and comparing that to a threshold.
The threshold is calculated based on a value between the black and white values typically (white - black) / 2.

In order for the threshold to be calculated correctly, the white and black values must be determined.  These values will change based on the lighting in the room and game mat so it is recommended to run a calibration routine ahead of each competitive match.

The python program below contains two functions.
- __do_calibration():__ Prompts the user to place the robot's color sensor above a black line and then a white line and records the refelected light intensity.  The values are then saved to a file for later use.
- __read_calibration():__ Retrieves the calibration values stored by the do_calibration() routine and returns them to the user as [black_value,white_value]

The program also contains example function calls.  Execution of the program will perform the do_calibration() and read_calibration() routines.

This approach is based off of the great example provided here: [https://github.com/bashir2/penguins_fll_2021](https://github.com/bashir2/penguins_fll_2021)
```python
#!/usr/bin/env pybricks-micropython

######################################################
# Basic setup for FSS FLL Spring Training Robot
# !!! -- DO NOT CHANGE THIS PART OF THE PROGRAM -- !!!
######################################################

# Import the necessary libraries
import math
import time
import os
from pybricks.ev3devices import *
from pybricks.parameters import *
from pybricks.robotics import *
from pybricks.tools import wait
from pybricks.hubs import EV3Brick
from pybricks.media.ev3dev import Font
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

# Read the color sensor Calibration
def read_calibration():
    '''
    Reads the previously stored calibration values
    and returns [black_value,white_value] as the lower
    and upper bounds of the reflected light
    '''

    # Setup the Font Type and Size
    ev3.screen.set_font(Font(None,20))

    # Wipe the EV3 display
    ev3.screen.clear()
    
    # Open the Calibration File
    try:
        file_handler = open("Calibration.txt", "r")
        black_value = int(file_handler.readline())
        white_value = int(file_handler.readline())
        ev3.screen.draw_text(0, 0,  "Retrieving Stored")
        ev3.screen.draw_text(0, 20, "Calibraion Values:")
        ev3.screen.draw_text(0, 60, "BLACK:" + str(black_value))
        ev3.screen.draw_text(0, 80, "WHITE:" + str(white_value))
    except OSError:
        # Calibration File was not found.  Use default
        # values of 0 and 100
        ev3.screen.draw_text(0, 0,  "Calibration.txt")
        ev3.screen.draw_text(0, 20, "Does not exist")
        ev3.screen.draw_text(0, 40, "Using Default Values")
        black_value = 0
        white_value = 100
        ev3.screen.draw_text(0, 60, "BLACK:" + str(black_value))
        ev3.screen.draw_text(0, 80, "WHITE:" + str(white_value))
    file_handler.close()
    wait(2000)
    return black_value, white_value
   
def do_calibration():
    '''
    Calibration routine for color sensor on port #1
    '''
    # Set the speaker Volume
    # Volume is set as a percentage of maximum
    # '_all_' signifies this should be used for all types of speaker sounds
    percent_volume = 100
    ev3.speaker.set_volume(percent_volume,'_all_')

    # Setup the voice characteristics
    # 'm1' 'm2' 'm3' are male voices
    # 'f1' 'f2' 'f3' are female voices
    # speed sets the words spoke per minute
    ev3.speaker.set_speech_options(language=None, voice='f2', speed=200, pitch=None)

    # Setup the Font Type and Size
    ev3.screen.set_font(Font(None,20))
    # Write your program here.
    ev3.screen.draw_text(0, 0, "Place Color Sensor")
    ev3.screen.draw_text(0, 20, "On Black")
    ev3.speaker.say("Place Color Sensor on Black")

    # Loop until the center button in pressed
    # When button pressed, the current value becomes the black value
    center_button_pressed = False
    while center_button_pressed == False:
        ev3.screen.draw_text(0, 0, "Place Color Sensor")
        ev3.screen.draw_text(0, 20, "On Black")
        ev3.screen.draw_text(0,60, "Current Value:" + str(ColorSensor(Port.S1).reflection()))
        wait(100)
        ev3.screen.clear()
        if(ev3.buttons.pressed() == [Button.CENTER]):
            center_button_pressed = True
            black_value = ColorSensor(Port.S1).reflection()
        else:
            center_button_pressed = False
    ev3.screen.draw_text(0, 0, "Place Color Sensor")
    ev3.screen.draw_text(0, 20, "On White")
    ev3.speaker.say("Place Color Sensor on White")

    # Loop until the center button in pressed
    # When button pressed, the current value becomes the white value
    center_button_pressed = False
    while center_button_pressed == False:
        ev3.screen.draw_text(0, 0, "Place Color Sensor")
        ev3.screen.draw_text(0, 20, "On White")
        ev3.screen.draw_text(0,60, "Current Value:")
        ev3.screen.draw_text(0,60, "Current Value:" + str(ColorSensor(Port.S1).reflection()))
        wait(100)
        ev3.screen.clear()
        if(ev3.buttons.pressed() == [Button.CENTER]):
            center_button_pressed = True
            white_value = ColorSensor(Port.S1).reflection()
        else:
            center_button_pressed = False

    # Wait for the user to stop pushing the button
    while ev3.buttons.pressed() == [Button.CENTER]:
        wait(100)

    # Write the white and black values into the calibration file
    center_button_pressed = False
    ev3.screen.draw_text(0, 0, "The Stored Calib")
    ev3.screen.draw_text(0, 20, "Values are")
    ev3.screen.draw_text(0,60, "BLACK:" + str(black_value))
    ev3.screen.draw_text(0,80, "WHITE:" + str(white_value))
    ev3.screen.draw_text(0, 100, "Press cntr btn to exit")
    wait(100)
    while center_button_pressed == False:
        if(ev3.buttons.pressed() == [Button.CENTER]):
            center_button_pressed = True
        else:
            center_button_pressed = False
    ev3.screen.clear()

    # Write Values to a file for use later:
    file_handler = open("Calibration.txt", "w")
    str_value = '{}\n{}'.format(black_value, white_value)
    file_handler.write(str_value)
    file_handler.close()

########################################
# do_calibration()
# Perform sensor calibration routine
#
# This is an example of the function call 
# that would be run as needed by the team 
# ahead of a competition match
#########################################
do_calibration()

########################################
# read_calibration()
# Open file and read calibration Values
#
# This is an example of how to read the
# stored calibration values.  This would
# normally be called a part of running
# a mission. 
########################################
[read_black,read_white] = read_calibration()
print("Black/Low Color Sensor Value = " + str(read_black))
print("White/High Color Sensor Value = " + str(read_white))


```
