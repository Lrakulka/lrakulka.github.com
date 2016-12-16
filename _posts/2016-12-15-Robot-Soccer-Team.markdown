---
layout: post
title:  "Robot Soccer Team"
date:   2016-12-15 00:00:00 +0100
categories: NAO robots soccer
---

*Report of my team*

Abstract—The following article is a short summary of work the ”Grand Dukes of Luxembourg” 
done during Autonomous Robot Software course during winter semester 2017.

**I. INTRODUCTION**

The winter 2017 Autonomous Robot Software course consisted of two parts.
First one, theoretical, was comprised of various lectures introducing us to the field of robotics and ROS (Robot
Operating System), as well as presenting the robotics platform - sensing, actuation and robot control.
It was closely intertwined with the other one - practical. Its main aim was to teach us to program NAO robots so that we
could program two to play a series of penalty kicks, trying to score goals and prevent the opponent from scoring one.
This report mostly focuses on practicalities, explaining how our work looked like and what the end results were, although
it also mentions how useful the theoretics were.

**II. AIMS OF THE PROJECT**

The main aims of the lecture were:
• Teach the students the fundamentals of robotic vision, sensing, localisation, navigation and intelligent controls.
• Provide students with some hands-on experience with ROS software
• Provide students with hands-on experience with the latest versions of the robot soccer software from the 
the worlds best robot soccer team today: ”B-Human”
• Provide students with an overview of robotics soccer competitions, particularly the RoboCup Standard Platform League.
• Produce working code for two roles required for a penalty match - Goalie (Robot that stands in front of the goal
and attempts to intercept all incoming shots) and Striker(Robot that attempts to score a goal)

**III. LEARNING PROCESS**
The entire course took a single semester and finished with a small, friendly tournament where the two teams let loose their
robots at each other. Yet it took some time to achieve level of comprehension allowing for such a match. 
Before taking part in the course
we had little to none practical experience with NAO robots, and the theoretical basics were lacking as well. Luckily, these
three months had proven to be enough to fill the gaps. The entire course began with lectures covering the basics
of robotics and current state of this field. The students were shown around University’s laboratories and experienced how
does the work on programming modern day robots look like. Lecture concerning basics of ROS was also included into this
part. It provided us with an understanding of basics of robotics, and a strong motivator for learning, as the drones 
made quite an impression on us.
Afterwards, we have moved on to more practical issue - teaching our robots how to play ball. At first we reviewed
most of the B-Human team documents and tutorials on our own, while receiving further lectures. Both the lectures and
the tutorials we were going over helped us understand how does the framework work, while documentation gave us some
understanding of how one solves problems related to football playing with robots.
The B-Human materials proved to be our constant companion. 
We had to spend another week or two to understand of how do the things work in the code, but we have eventually
managed to do it, with some advice from more experienced students and Dr. Cairie. Seeing how a lot of useful functions
and procedures were already in the files, this helped us immensely when programming our robots.
And this was precisely what we have gotten to next. We spent the next few weeks developing existing Striker role
and creating the Goalie role from scratch. Luckily enough, we did not have to submit the robots themselves to iteration
after iteration of our code; the B-Human package contained a simulator that allowed us to test even the craziest ideas without
risking bodily harm of the robots.
Eventually we have managed to implement the roles and create animations required for the robots to act as we ordered
