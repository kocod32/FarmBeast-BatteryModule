
## What is ROS?

ROS stands for **Robot Operating System**, although the name is slightly misleading. ROS is not a full operating system like Windows or Ubuntu. It is a **software framework for robotics** that helps us build robot applications in a structured and modular way.

In ROS 2, software is usually split into smaller parts called **nodes**. For example, one node may read data from a camera, another may control a robot arm, and another may visualize data or plan motion. These nodes can communicate with each other, which makes ROS useful for larger and more complex robotic systems.

For this project, ROS gives us a common environment for development, testing, and communication between software components.

## Why not use Windows directly?

ROS 2 can also be installed on Windows, so using Windows is possible. However, in practice, most ROS tutorials, tools, and package workflows are built mainly around **Ubuntu Linux**. Because of that, working directly on Windows often leads to more setup differences, more compatibility issues, and more difficult debugging.

So the main reason is not that Windows is wrong, but that Ubuntu is usually the more practical and more standard choice for ROS work, especially for beginners.

## Why do we use a virtual machine?

A **virtual machine (VM)** lets us run Ubuntu inside Windows. This means your laptop still uses Windows as the main operating system, but inside it you create a separate Ubuntu system. ROS 2 Jazzy is then installed inside that Ubuntu environment.

This approach is useful because it gives Windows users a Linux-based setup without needing to remove Windows or reinstall the whole laptop. It also means everyone can follow the same Ubuntu-based workflow, which makes installation and troubleshooting much easier.
<div style="page-break-before: always;"></div>

## VM setup

Before installing ROS, first create and configure the Ubuntu virtual machine by following this video:

**VM configuration video:**  
https://www.youtube.com/watch?v=DhVjgI57Ino

The goal of this step is to get a working Ubuntu virtual machine. Once that is ready, continue with the next section.

## Basic Linux terminal commands

Before installing ROS 2, it is important to understand the basics of the Linux terminal. Most of the ROS setup will be done through the command line, so users should be comfortable with simple Bash commands such as navigating between folders, listing files, creating directories, and running commands.

![[Screenshot from 2026-04-01 13-57-35.png]]

For a basic introduction, watch this video - The goal is only to understand the basics well enough to follow the ROS installation steps.

**Linux Bash basics:**  
https://www.youtube.com/watch?v=MCwAKGC2tkU&list=PLNWNEEf8BvG6LncOfJ6NinHFvMDQm9hA6&index=2


## Installing ROS 2 Jazzy on Ubuntu

Once your Ubuntu virtual machine is ready and you are comfortable with basic terminal commands, the next step is to install ROS 2 Jazzy.

Below is a simplified walkthrough of the main steps. You can also refer to the official ROS 2 Jazzy installation guide for Ubuntu:

**Official guide:**  
https://docs.ros.org/en/jazzy/Installation/Ubuntu-Install-Debs.html#install-ros-2


## System setup

Before installing ROS 2, first make sure the system locale is set correctly and that the required Ubuntu repositories are enabled.

Check the locale:

```bash
locale
```

If needed, run:

```bash
sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8
```

Then verify the locale again:

```bash
locale
```

Next, enable the required Ubuntu repositories:

```bash
sudo apt install software-properties-common
sudo add-apt-repository universe
```
<div style="page-break-before: always;"></div>


Now add the ROS 2 apt source:

```bash
sudo apt update && sudo apt install curl -y
export ROS_APT_SOURCE_VERSION=$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F "tag_name" | awk -F'"' '{print $4}')
curl -L -o /tmp/ros2-apt-source.deb "https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-apt-source_${ROS_APT_SOURCE_VERSION}.$(. /etc/os-release && echo ${UBUNTU_CODENAME:-${VERSION_CODENAME}})_all.deb"
sudo dpkg -i /tmp/ros2-apt-source.deb
```

It is also useful to install the development tools:

```bash
sudo apt update && sudo apt install ros-dev-tools
```

## Install ROS 2 Jazzy

Before installing ROS 2, update the package list and upgrade the system:

```bash
sudo apt update
sudo apt upgrade
```

Then install ROS 2 Jazzy:

```bash
sudo apt install ros-jazzy-desktop
```


## Test the installation

To check whether ROS 2 was installed correctly, open two terminals.

In the first terminal, run:

```bash
source /opt/ros/jazzy/setup.bash  
ros2 run demo_nodes_cpp talker
```

In the second terminal, run:

```bash
source /opt/ros/jazzy/setup.bash  
ros2 run demo_nodes_py listener
```

If the installation is correct, the talker will publish messages and the listener will receive them.

## Create a ROS 2 workspace

After installing ROS 2, the next step is to create your own **ROS 2 workspace**. This is the folder where you will keep your ROS packages and future projects.

Create the workspace and its `src` folder with:

```bash
# stay in home directory
mkdir -p ~/ros2_ws/src  
cd ~/ros2_ws
```

Then build the workspace:

```bash
colcon build
```

After the build finishes, source the workspace:

```bash
source ~/ros2_ws/install/setup.bash
```

This tells the terminal to also use packages from your own workspace, not only the main ROS 2 installation.

A ROS 2 workspace usually contains the following folders:

- `src` for source code and packages,
- `build` for generated build files,
- `install` for built packages,
- `log` for build logs.

At the beginning, only `src` is created manually. The other folders appear automatically after the first build.
<div style="page-break-before: always;"></div>

## Add ROS sourcing to `.bashrc`

In ROS 2, **sourcing** means loading environment settings into the current terminal session. This is important because it tells the terminal where ROS is installed and makes commands like `ros2` available.

To make this automatic every time you open a new terminal, add the source commands to your `.bashrc` file.

Open the `.bashrc` file in a text editor. If `gedit` is not installed yet, install it first:  
  
```bash  
sudo apt update  
sudo apt install gedit  
gedit ~/.bashrc    # open the editor
```

Then add these lines at the end:

```sh
source /opt/ros/jazzy/setup.bash  
source /usr/share/colcon_argcomplete/hook/colcon-argcomplete.bash  
source ~/ros2_ws/install/setup.bash
```

After saving the file, reload it with:

```bash
source ~/.bashrc
```

This will automatically load ROS 2 Jazzy, enable `colcon` autocomplete, and source your workspace if it has already been built.
<div style="page-break-before: always;"></div>

## Install Visual Studio Code

For writing code and working with ROS packages, it is recommended to install **Visual Studio Code** inside the Ubuntu virtual machine.  
  
To install it, follow this video:  
  
**VS Code installation video:**  
https://www.youtube.com/watch?v=NX8SHmkuLn4  
  
If needed, you can also install it manually:  
  
1. Go to the official download page:  
https://code.visualstudio.com/download  
  
2. Download the **Linux x64 .deb** package.  
  
3. Open a terminal in the folder where the file was downloaded and install it with:  
  
```bash  
sudo apt install ./<name-of-downloaded-file>.deb
```

For example, if the file is called `code_amd64.deb`, run:

```bash
sudo apt install ./code_amd64.deb
```

After installation, you can start VS Code from the Ubuntu application menu or by running:

```bash
code
```


## What next?

If you feel comfortable after the setup, you can continue with the official ROS 2 Jazzy tutorials:

**ROS 2 Jazzy tutorials:**  
https://docs.ros.org/en/jazzy/Tutorials.html

They cover beginner topics such as CLI tools, workspaces, packages, nodes, topics, services, launch files...

