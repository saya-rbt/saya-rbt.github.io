---
title: "HAT: Human Aware Turtlebot - The writeup"
date: 2021-06-14T11:42:28+02:00
draft: true
tags: ["Robots", "HRI", "Nintendo 3DS", "Tensorflow", "Pose estimation", "Teachable Machine", "ROS"]
---

## Context

**Human Aware Turtlebot** (also named **HAT** or **HATb**) was my master's degree final class project, where I had 8 weeks (but since I was in an integrated work and study program, the actual amount of time I had fully dedicated to that project was half that, so 4 weeks) to make a project based on what we had seen this semester. My classmates and I could almost choose the entire scope of our project as well as the classmates we wanted to work on, and it was our responsibility to decide of our goals and steps with help from our teachers if needed.

The projects being held during the COVID-19 crisis, we couldn't go back to the school buildings as regularly as usual, so some projects had to be made on simulators (such az [Gazebo](http://gazebosim.org/)). We had the opportunity to do something with actual robots, so I initially wanted to do something regarding interaction with one of the [Pepper](https://www.softbankrobotics.com/emea/en/pepper) or [Nao](https://www.softbankrobotics.com/emea/en/nao) the school had. Unfortunately, at the time my project was supposed to start, they were all out of order. Therefore, I decided to use a [Turtlebot2i](https://www.trossenrobotics.com/interbotix-turtlebot-2i-mobile-ros-platform.aspx) without the articulate arm, though in retrospect it could've been interesting to use it. I had a little experience in working with the Turtlebot since I had classes on [ROS](https://www.ros.org/) using it as an actual application of the platform.

The initial description of the project goals was as followed:

> The main goal is to have a Turtlebot being able to navigate in a room and react to people's interactions. For instance, if someone is crouching and looking towards the Turtlebot, the Turtlebot will have to come to the person while avoiding collisions, or if the person is standing close to the bot with insistance, the bot will have to move out of the way, or if someone is looking at the bot and pointing to something/somewhere, the bot will have to go there.
>
> Optionally, other interesting aspects could be to have the bot carry items on top of it (and take that into account in its pathfinding not to make the objects fall), have a screen to make the bot more personable, maybe add a "manual control" switch where someone can use a web app to control the bot with a controller or a keyboard and have the video feedback from the kinect streamed to the "control center", etc, but since those are less about the bot's interactions with the human and more about the controlling of the bot itself, it's more of a stretch goal than anything.