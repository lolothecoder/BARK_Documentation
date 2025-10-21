Software Setup
==============

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

.. include:: _sidebar.rst