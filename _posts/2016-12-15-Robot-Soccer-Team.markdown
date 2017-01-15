---
layout: post
title:  "Robot Soccer Team"
date:   2016-12-15 00:00:00 +0100
categories: NAO robots soccer
---

***Test of our goalkeeper***

[![Grand Dukes of Luxembourg tests their goalkeeper](https://i.ytimg.com/vi/MgTiuLhjhVA/hqdefault.jpg?custom=true&w=196&h=110&stc=true&jpg444=true&jpgq=90&sp=68&sigh=O3j8V7c3iC5Kyok3hxWLNkQGrlA)](https://youtu.be/MgTiuLhjhVA)

***Report of my team***

Abstract—The following article is a short summary of work the ”Grand Dukes of Luxembourg” done during Autonomous Robot Software course during winter semester 2017.

![Our Team](/images/team.png)

**I. INTRODUCTION**

The winter 2017 Autonomous Robot Software course consisted of two parts.
First one, theoretical, was comprised of various lectures introducing us to the field of robotics and ROS (Robot Operating System), as well as presenting the robotics platform - sensing, actuation and robot control.
It was closely intertwined with the other one - practical. Its main aim was to teach us to program NAO robots so that we could program two to play a series of penalty kicks, trying to score goals and prevent the opponent from scoring one.
This report mostly focuses on practicalities, explaining how our work looked like and what the end results were, although it also mentions how useful the theoretics were.

**II. AIMS OF THE PROJECT**

The main aims of the lecture were:

1. Teach the students the fundamentals of robotic vision, sensing, localisation, navigation and intelligent controls.
2. Provide students with some hands-on experience with ROS software
3. Provide students with hands-on experience with the latest versions of the robot soccer software from the the worlds best robot soccer team today: ”B-Human”
4. Provide students with an overview of robotics soccer competitions, particularly the RoboCup Standard Platform League.
5. Produce working code for two roles required for a penalty match - Goalie (Robot that stands in front of the goal and attempts to intercept all incoming shots) and Striker(Robot that attempts to score a goal)

**III. LEARNING PROCESS**

The entire course took a single semester and finished with a small, friendly tournament where the two teams let loose their
robots at each other. Yet it took some time to achieve level of comprehension allowing for such a match. 

![Teams hard at work](/images/title23375445.png)

Before taking part in the course we had little to none practical experience with NAO robots, and the theoretical basics were lacking as well. Luckily, these three months had proven to be enough to fill the gaps. The entire course began with lectures covering the basics of robotics and current state of this field. The students were shown around University’s laboratories and experienced how does the work on programming modern day robots look like. Lecture concerning basics of ROS was also included into this part. It provided us with an understanding of basics of robotics, and a strong motivator for learning, as the drones made quite an impression on us.

![Life test](/images/WP_20161208_14_21_51_Pro_LI.jpg)

Afterwards, we have moved on to more practical issue - teaching our robots how to play ball. At first we reviewed most of the B-Human team documents and tutorials on our own, while receiving further lectures. Both the lectures and the tutorials we were going over helped us understand how does the framework work, while documentation gave us some understanding of how one solves problems related to football playing with robots.
The B-Human materials proved to be our constant companion. We had to spend another week or two to understand of how do the things work in the code, but we have eventually managed to do it, with some advice from more experienced students and Dr. Cairie. 
Seeing how a lot of useful functions and procedures were already in the files, this helped us immensely when programming our robots. And this was precisely what we have gotten to next. We spent the next few weeks developing existing Striker role and creating the Goalie role from scratch. Luckily enough, we did not have to submit the robots themselves to iteration after iteration of our code; the B-Human package contained a simulator that allowed us to test even the craziest ideas without risking bodily harm of the robots.
Eventually we have managed to implement the roles and create animations required for the robots to act as we ordered them to. Only afterwards had we moved from the simulator to NAOs, and confronted simulated results with real life. 
After spending another few hours on fine-tuning the code we have finally managed to achieve satisfactory results. 
A day later the teams have met on the green grass for the friendly match.
To top it all off, we have received an interesting lecture concerning topics we had not necessarily needed during our development process - Cognitive AI and Maps, localisation and navigation.

**IV. RESULTS**

All in all, we have managed to complete all of the tasks we had planned. We have received solid, theoretical background, allowing us to develop ourselves in the field of robotics. We were taught the basics of sensing, intelligent controls, robotic vision and navigation, and some ideas concerning Cognitive AI and maps were explained to us.
We have played around with ROS software and learnt from the best as to how to program a NAO robot for playing
football. Finally, we have managed to program the roles of Striker and Goalie. And all of in a pleasant atmosphere of fun and companionship.

**V. ISSUES AND POSSIBLE DEVELOPMENTS**

There were quite a few issues we had to face while developing our application. Save for most obvious problems
that always appear when getting into a new project, there were a few that required hard decisions to be made.
Firstly, there was the issue of picking a role for our robot. At first we have had the robot look around, and decide which
role should he take depending on distance to the ball and characteristic points. This proved fairly impractical, though,
as the robot had to go through a bootstrap phase to measure required distances. Eventually we have let the decision be
made based on the robot ID. This is probably the best option, and will remain applicable even during ”big” matches, at least
for initial set-up.

```C
Listing 1. PlayingState.h

option (PlayingState)
{
  initialstate(play)
  {
    transition
    {
      if (theRobotInfo.number != 1 ) {
      Striker();
    } else {
      Goalie () ;
    }
   }
 }
}
```

Another thing we have had to decide on was to how to make the Goalie detect the incoming shot. At first we were attempting to measure change of distance to the ball in two consecutive moments, but this meant that we have had to introduce our own timers and sleeps for it to be possible. While the timer idea (which checks how much time has passed since the last test, and if enough - measures the distance to the ball again and compares it with previous one) held some promise, we found trying to implement it impractical. Eventually we have decided to rely on simple distance to the ball, seeing how the ball usually will not approach enough for the goalie to react in course of normal play.

```C
Listing 2. Goalie.h
if ( theBallModel.estimate.position.norm() < 1100.f ) // If the ball is close enough
  goto defendTheShot;
// It means, that the shot has been
/ / made and it should be defended
```

Striker had his own issues, and they have shown themselves only after we moved to real life.
First of them were the problems with kick strength. While creating the animation responsible for kick, we have easily
managed to modify the parameters to allow for a strong kick. Surprisingly, these changes caused robot to fall, whenever
kicking. It turned out, that rather than modify the kick itself, we have altered the angle at which the striker held his feet. In simulator this translated into greater strength imparted on the ball, but it also caused this toppling behaviour with the striker. 
Another issue, was related to lighting. At first, the striker detected both the ball and the goal, and then attempted to set up the shot accordingly. This behaviour was preserved on the master branch of our repository. As it soon turned out, in real life the robot was usually unable to notice the ball. This made him approach it, and then start looking around in search of goal - which usually lead to him accidentally kicking the ball sideways.
Seeing how we had little influence on the lighting, we had to dumb the robot down. Since during kickoffs the kicker will
be set up so as to have both the ball and goal in front of him, all we left in was setting up behind the ball and kicking it.
This gave much improved results, although it is more of a stopgap solution. To be quite honest, there are more possible developments than actual code. While we have learnt a lot while working on NAOs, there is a lot more that could be done.
As far as Goalie is concerned, two things that could be done would be teaching him to defend without falling down, and to
get ready for another defence after punching the ball away. Striker is working fine as is, although some thought could
be given to him aiming at a place in opponent’s goal for the ball to give as wide a berth as possible to opponent’s goalie.
And there are still other roles to implement. If the robots were to take place in actual robot’s Olympics, they would need
to dribble with opponents, pass the ball between each other and perhaps form a rudimentary strategy. There are a lot of possible avenues of development, and seeing how programming such robots is a lot of fun, someone is bound to explore them further soon.

**VI. TEACHING AIDS**

Save for materials provided by Professor Voos Holger and Doctor Patrice Caire, we have used following sources while
working on our robots:

1. [Video tutorials from Robocum ETH team](https://www.youtube.com/playlist?list=PLX5LVYI-ZPJvrVhLjUKJLD9gZsKthsAKB)
2. [Code used by B-Human](https://www.b-human.de/downloads/publications/2013/CodeRelease2013.pdf)
