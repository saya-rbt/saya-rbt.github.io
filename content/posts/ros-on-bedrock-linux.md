---
title: "ROS on Bedrock Linux"
date: 2021-08-14T16:00:37+02:00
draft: false
tags: ["Linux", "Bedrock Linux", "ROS", "Robots", "Ubuntu", "Noetic"]
---
This post is a record of all the tweaks I've had to use to install and run ROS on Bedrock Linux, as well as issues I've had to face and the solutions I found for it.

**Note: this post is prone to editions, updates etc as I encounter more issues or have more tips to share. It is also important to note that I am in no way affiliated with ROS, Bedrock Linux or anything, I just happen to be trying it and thought I'd share anything I might deem useful for other people if they decide to run ROS on Bedrock Linux too. I don't know both of this tools well enough to provide advanced support, but you can always contact me for any questions!**

## Installation

### Fetching the correct Ubuntu release

ROS works best with an Ubuntu/Debian release it matches. I've personally been using ROS Noetic, recommended for Ubuntu 20.04.

Ubuntu 20.04's LTS release full name being **Focal Fossa**, the release name is `focal`. Therefore, I installed an Ubuntu `stratum` using `brl fetch`:

```
$ brl fetch ubuntu -r focal
```

You can also, optionally, give this stratum a specific name in order to differentiate it from your other stratas, with the `-n` flag.

If you plan on using a different ROS release, please make sure to have the correct Ubuntu LTS release as well.

You can list available Ubuntu releases with the following command :

```
$ brl fetch ubuntu --releases
```

And check what LTS release you need for your ROS distribution by checking the [ROS Wiki](http://wiki.ros.org/ROS/Installation).

*Note:* If you prefer to use Debian over Ubuntu, you can do so simply by replacing `ubuntu` by `debian` and `focal` by the release name (for ROS Noetic, `buster`)

### Installing ROS

Once you have your stratum set up, you can simply carry on with the [regular ROS installation process](http://wiki.ros.org/noetic/Installation/Ubuntu).

The only thing you have to do is prefix every command with `strat ubuntu` (or `sudo strat ubuntu`). **Note: this might not be mandatory (I haven't tried because it just didn't feel like a good idea), but it's best to do so in order to avoid any cross-stratum mishappenings.**

## Usage

### Running `roscore`

Remember what I just told you about installing ROS specifically on the Ubuntu stratum? Well, using it is the same. You can just prefix it with the `strat` command:

```
$ strat ubuntu roscore
```

Even better yet, you can restrict the ROS environment to Ubuntu's environment only:

```
$ strat -r ubuntu roscore
```

Doing so will prevent ROS from seeing cross-stratum hooks, and therefore should make it more stable.

You can find more information on [Bedrock Linux's documentation](https://bedrocklinux.org/0.7/basic-usage.html).

### Running any other ROS command

Running any ROS command works the same way as `roscore`, so just run it on your Ubuntu stratum (and optionally restrict it).

### Optional: transparent ROS integration to Bedrock Linux

If you don't want to bother with using the `strat` command every time you want to run a ROS command, you can always define an alias that will do it for you:

```bash
alias ros="strat -r ubuntu ros"
```

For more details or aliases, you can check mine in the [Aliases](#aliases) section.

## Common issues and problems

### `roscore` returns `ModuleNotFoundError: No module named 'defusedxml'`

If, when running `roscore`, you get the following error (or similar):

```
$ roscore
Traceback (most recent call last):
  File "/bedrock/strata/ubuntu/opt/ros/noetic/bin/roscore", line 36, in <module>
    from rosmaster.master_api import NUM_WORKERS
  File "/bedrock/strata/ubuntu/opt/ros/noetic/lib/python3/dist-packages/rosmaster/__init__.py", line 35, in <module>
    from .main import rosmaster_main
  File "/bedrock/strata/ubuntu/opt/ros/noetic/lib/python3/dist-packages/rosmaster/main.py", line 43, in <module>
    import rosmaster.master
  File "/bedrock/strata/ubuntu/opt/ros/noetic/lib/python3/dist-packages/rosmaster/master.py", line 47, in <module>
    import rosmaster.master_api
  File "/bedrock/strata/ubuntu/opt/ros/noetic/lib/python3/dist-packages/rosmaster/master_api.py", line 72, in <module>
    from rosmaster.util import xmlrpcapi
  File "/bedrock/strata/ubuntu/opt/ros/noetic/lib/python3/dist-packages/rosmaster/util.py", line 48, in <module>
    from defusedxml.xmlrpc import monkey_patch
ModuleNotFoundError: No module named 'defusedxml'
```

You've simply been running ROS without specifying the correct stratum, making ROS confused since it can not find the `defusedxml` library.

Just run it again by specifying the correct stratum with `strat [-r] ubuntu roscore` and you should be fine.

## Aliases

Here's a list of the aliases I use for running ROS on Bedrock Linux. This list is non-exhaustive and I will add things as I experiment more:

```bash
# brl ros aliases
## brl ros prefix: just use like brlros core to run roscore, for instance
## you could rename it ros instead of brlros to have a seamless 
## ROS integration to Bedrock Linux, but I personally don't for clarity
function brlros { strat -r ubuntu ros$*; }
```