SimplePublisher
===============

This is a program that allows you quickly send commands to a ROS Node.

//#!/usr/bin/env python

from rospy import init_node, is_shutdown

from std_msgs.msg import Float32MultiArray

import rospy

array = Float32MultiArray()

run_once = 0

frequency = 1

nums = 1.0

//#nums = raw_input('Input Nums: ').split(" ")

if __name__ == '__main__':

    #Define Publisher and Initialize Node
    pub = rospy.Publisher('float_pub',Float32MultiArray,queue_size=10)
    init_node("robot", anonymous=True)
    #Take User input to decide publish rate
    try:  
        frequency = input('Publish frequency: ')
    except:
        pass
    run_once = 1
    r = rospy.Rate(frequency)
    r.sleep()
    while not rospy.is_shutdown(): 
        #Asks user for any number input 
        nums = raw_input('Input Nums: ').split(" ")
        for item in nums:
            #If you tpe publish you can redefine the publish rate
            if(item == "publish"):   
                frequency = input('Publish frequency: ')
                r = rospy.Rate(frequency)
            else:
                pass
            try:
                """Here it changes each item in list "nums" to ints and if
                you accidently type in letters in between it will remove
                the word and then continue publishing numbers"""
                item = int(item)
                if(type(item) != int):
                    item.remove(item)
            except ValueError, UnboundLocalError: 
                pass
            """Then if "item" is a int then it tries to convert it to a
            float and publishes it in index [0] of Float32MultiArray"""
            if(type(item) == int):
                item = float(item)  
                array.data  = [item,0.0]
                r.sleep()
                pub.publish(array)
