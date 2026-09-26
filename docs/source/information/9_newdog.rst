Byte Main Page
==============

Purpose
~~~~~~~
The goal of Byte is to be an assistive dog. It should help a blind person in their everyday tasks.
We want to inspire us from this project:

Thesis Tazer: https://www.youtube.com/watch?v=L8qSAampsHU&t=534s

Requirements
~~~~~~~~~~~~
- The robot shall have a height of between 40 and 55 cm. This is about the normal size for robot dogs.
- Each leg shall have 3 Degrees of Freedom
- The robot should be able to be tele-operated
- The robot should be able to move autonomously
- The robot should be able to recognize objects in its surrounding
- The robot should be safe to use around humans

Brainstorming Electrical/Sensor Requirements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- 2x 6S batteries
- Plan for modularity
- LIDAR
- Camera
- Speaker + Mic
- GIM8010-8 Motors + drivers
- Power circuitry
- Jetson Orin Nano
- AI accelerator (GPU)
- IMU
- Red emergency button

Connecting to the Jetson
~~~~~~~~~~~~~~~~~~~~~~~~
To connect to the Jetson, first make sure you are connected to the ``spot-iot`` Wi-Fi network, then run:

.. code-block:: bash

   ssh byte@172.21.67.198

The password is ``woof1234*``.