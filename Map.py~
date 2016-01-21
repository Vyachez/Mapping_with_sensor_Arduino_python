# -*- coding: cp1251 -*-
#Arduino Python map plotting by Vyachez
import matplotlib.pyplot as plt
import math
import time
import os
import sys
from Arduino import Arduino

#Enabling pin to switch the relay for empowering servo and sensor
sw_2 = 50;

#Operating pin for servo
SerP = 6;

#Central angle for servo
Cpoint = 100

#Angle list for servo
angle = [0, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55, 60, 65, 70, 75, 80, 85, 90, 95, 100, 105, 110, 115, 120, 125, 130, 135, 140, 145, 150, 155, 160, 165, 170, 175, 180, 180]

#checking even length of lists
if (len(angle) % 2 != 0):
        print "error - the list of angles provided is not even!"
        time.sleep(0.5)
        print "exiting the program............................."
        time.sleep(1)
        sys.exit()
else:
    print "ok angle length is: ", len(angle)

def block2_switch(switch2):
    board.pinMode(sw_2, "OUTPUT")
    board.digitalWrite(sw_2, switch2)

class Arduino(Arduino):

    def ultraRead(self):
        """ Read signal from a Ultrasonic sensor."""
        cmd_str = "@{cmd}%$!".format(cmd="ultra")
        try:
            self.sr.write(cmd_str)
            self.sr.flush()
        except:
            pass
        rd = self.sr.readline().replace("\r\n", "")
        return rd

board = Arduino("921600", "/dev/ttyACM0")
time.sleep(1) #let Arduino to init

block2_switch("LOW") # enable servo and sensor
time.sleep(0.5)

board.Servos.attach(SerP) #attaching servo

#reading the sensor and small test
print "program started: ", str(board.ultraRead()), " = test distance"
print "please wait..."
time.sleep(0.3)
board.Servos.write(SerP, 0)
time.sleep(1.5)

distance = []

def ranging():
    angl = 0
    for i in range(38):
        board.Servos.write(SerP, angl)
        time.sleep(0.1)
        ul_read = str(board.ultraRead()) # reading data from sensor
        distance.append(ul_read)
        angl += 5
        time.sleep(0.1)
    return distance

def gettin_y(dist, ang):
    y_list = []
    for i in range(len(dist)):
        a = float(dist[i]) * math.sin(math.radians(float(ang[i])))
        y_list.append(float("{0:.2f}".format(a)))
    return y_list
    
def gettin_x1(dist, ang):
    midp = int(len(dist)/2)
    x1_list = []
    for i in range(0, midp):
        b1 = abs(float(dist[i]) * math.cos(math.radians(float(ang[i]))))+ Cpoint
        x1_list.append(float("{0:.2f}".format(b1)))
    return x1_list

def gettin_x2(dist, ang):
    midp = int(len(dist)/2)
    x2_list = []
    for i in range(midp, len(dist)):
        b2 = Cpoint - abs(float(dist[i]) * math.cos(math.radians(float(ang[i]))))
        x2_list.append(float("{0:.2f}".format(b2)))
    return x2_list

time.sleep(0.2)
ranging()
time.sleep(0.2)

x1 = gettin_x1(distance, angle)
x2 = gettin_x2(distance, angle)
x = x1 + x2
y = gettin_y(distance, angle)

time.sleep(0.2)
midp = int(len(distance)/2)

print "distance: ", distance, "len: ", len(distance)
print "x = ", x, "len = ", len(x)
print "y = ", y, "len = ", len(y)

print "midpoint: ", midp

print "x to midp: ", x1,"len: ", len(x1)
print "+++++++++++"
print "midp to x", x2, "len: ", len(x2)

board.Servos.write(SerP, 90)
distance = []
time.sleep(1.5)
block2_switch("HIGH") # disable servo and sensor

#Showing the plot
time.sleep(0.2)
plt.plot(x, y, 'bo')
plt.axis([0, int(Cpoint*2), 0, int(Cpoint*2)])
plt.show()
