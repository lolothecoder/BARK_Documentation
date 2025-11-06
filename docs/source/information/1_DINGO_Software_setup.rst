DINGO Software Setup
====================

To connect to the Pi make sure you are on the `SPOT-iot wifi <https://make.epfl.ch/tools/iot-wifi.php/>`_.

Then to ssh onto the Pi type:

.. code-block:: bash

   ssh adam@172.21.64.184

**Password:** adam

Running the DINGO with ROS (From a ubuntu computer)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Make sure the servos are properly calibrated and that the right offset matrix is in HardwareInterface.py. If unsure please calibrate the servos by following
the steps outlined in CalibrateServos.py

Connect to the Pi with

.. code-block:: bash

   ssh -X adam@172.21.64.184

**Password:** adam

The -X is important and allows the keyboard to work. Then execute

.. code-block:: bash

   roslaunch dingo dingo.launch is_physical:=1 is_sim:=0 use_joystick:=0 use_keyboard:=1

Press 1 to enter TROT mode and press backspace or 2 to go into REST mode. In TROT mode the legs will start moving but the DINGO will stay in place. To get
it moving press W to give him a forward speed

You can view the commands being sent to DINGO by opening up a new terminal and launching

.. code-block:: bash

   rostopic echo /joy


How to use the docker container on a laptop
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**CAREFUL: You should not follow the same instructions on the raspberry PI, this is for laptops and everything is calibrated such that the simulation works correctly.
First, you need to have a linux OS. If you have apple silicon chip, use https://asahilinux.org/, if you have x86 chip, use an ubuntu dual boot for example (it can be fedora or debian too but preferably ubuntu).**

Go into terminal:

.. code-block:: bash

   git clone <https://github.com/Yerbert/DingoQuadruped.git>
   cd DingoQuadruped
   code .

You should see two important files: dockerfile and Docker-compose.yaml:

- Dockerfile: Its goal is to give the intructions to build a docker image. The instructions are bash instructions used to intall libraries, certain dependencies, set the OS version, set the python version, …
- Docker-compose.yaml: build images based on the dockerfiles and launch containers and associate volumes with them, …

Firstly, open the dockerfile and replace its content with the following:

.. code-block:: bash

   FROM ros:noetic-robot 
   RUN sudo sh -c 'echo "deb <http://packages.ros.org/ros/ubuntu> $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
   RUN sudo apt install curl     RUN curl -s <https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc> | sudo apt-key add -

   RUN apt-get update && \\    apt-get install -y --no-install-recommends \\    gdb \\    apt-utils \\    python3-rosdep \\    python3-pip \\    python3-vcstool \\    python3-pymodbus \\    build-essential \\    ros-noetic-catkin \\    python3-catkin-tools \\    ros-noetic-ros-controllers \\    nano \\    ros-noetic-soem \\    libvlccore-dev \\    libvlc-dev \\    ros-noetic-joy \\    ros-noetic-rosserial \\    ros-noetic-rosserial-arduino \\    git 

   RUN apt update && apt install -y x11-apps 

   RUN pip3 install \\    #Following are from pupper codexeyes    transforms3d \\    UDPComms \\    serial \\    pyserial \\    pigpio \\    regex \\    matplotlib \\    #Following are Nathan/Alex additions    pynput \\    spidev \\    #adafruit-circuitpython-pca9685 \\    adafruit-circuitpython-servokit

   RUN apt-get updateRUN apt-get install -y ros-noetic-gazebo-ros-pkgs ros-noetic-gazebo-ros-control

   WORKDIR /dingo_wsCOPY /dingo_ws/src /dingo_ws/srcRUN rosdep updateRUN rosdep install --from-paths src --ignore-src -r -y
   RUN echo "source /opt/ros/$ROS_DISTRO/setup.bash" >> /etc/bash.bashrc
   RUN /bin/bash -c 'source /opt/ros/$ROS_DISTRO/setup.bash &&\\catkin_make --directory /dingo_ws -DCMAKE_BUILD_TYPE=Debug'
   RUN echo "source /opt/ros/$ROS_DISTRO/setup.bash" >> /etc/bash.bashrcRUN echo "source /dingo_ws/devel/setup.bash" >> /etc/bash.bashrc
   ENV DISPLAY=:0 
   ENV LIBGL_ALWAYS_SOFTWARE=1
   CMD [ "bash" ]

This step is very important. Indeed, the original dockerfile is missing some important stuff to run the project correctly on a computer and the image osrf:ros doesn’t work on arm.
Then at the root of DingoQuadruped, run:

.. code-block:: bash

   sudo docker compose build --no-cache

The —no-cache flag is very important st it builds from the beginning
Once this step is done, we can use the image to launch containers.

.. code-block:: bash

   sudo docker compose up

To identify the containers launched, open a new terminal and type:

.. code-block:: bash

   sudo docker ps

You’ll get a list of containers. Take the ID of the most recent one and type:

.. code-block:: bash

   sudo docker exec -it e78a4f62df13 bash

e78a4f62df13 is an example of CONTAINER ID you’ll have
Now you can type commands in your container.
If you try to test the linkage between the display and the container for graphical purpose, you can type:

.. code-block:: bash

   xeyes

You’ll probably get:

.. code-block:: console

   root@adam-macbookair:/dingo_ws# xeyes
   Authorization required, but no authorization protocol specified
   Error: Can't open display: :0

Open a new terminal and type:

.. code-block:: bash

   xhost +

Then try again xeyes. If it still doesn’t work, ask god for help.

Trying the simulation:
~~~~~~~~~~~~~~~~~~~~~~

Before using the project, you’ll need to compile it:

.. code-block:: bash

   cd dingo_ws
   catkin_make
   source devel/setup.bash

In the Dingo project, to launch the simulation, you’ll need to launch the gazebo tools and the dingo nodes. Open two different terminals and access the container (the same one of course, as before)
To launch the gazebo tools in the first terminal:

.. code-block:: bash

   roslaunch dingo dingo_simulator.launch

You should see a gazebo window appearing
To launch the dingo nodes, use:

.. code-block:: bash

   roslaunch dingo dingo.launch is_sim:=1 is_physical:=0 use_keyboard:=1 use_joystick:=0

You can use up down left and right keys to test everything works correctly.

Flashing Arduino
~~~~~~~~~~~~~~~~

A sketch is actually a folder where there is a .ino file of the same name.

To compile the code:

.. code-block:: bash

   arduino-cli compile --fqbn arduino:avr:nano MySketch

To put it into the arduino

.. code-block:: bash

   arduino-cli upload -p /dev/ttyUSB0 --fqbn arduino:avr:nano MyFirstSketch

If you followed the steps correclty, arduino-clli should look for the libraries into

.. code-block:: console

   ~/Arduino/libraries

Thus, if you need to add specific libraries etc, add them there

.. include:: _sidebar.rst