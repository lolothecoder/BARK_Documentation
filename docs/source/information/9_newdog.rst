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

Football Demonstrator
~~~~~~~~~~~~~~~~~~~~~
This is the demonstrator that we want to do: play football with the leg that we built.

.. image:: /assets/byte_research/footbyte.jpeg
   :width: 800px
   :align: center

|

Test Setup Instructions
~~~~~~~~~~~~~~~~~~~~~~~
Here is a diagram of the wiring for the test setup:

.. image:: /assets/byte_research/NewTestSetupWiring.png
   :width: 800px
   :align: center

|

And the real implementation:

.. image:: /assets/byte_research/New_Test_wiring.jpeg
   :width: 800px
   :align: center

|

**Step 1**

Plug in the motors and the Jetson to the PDBueno

**Step 2**

Make sure the main switch is OFF and the Precharge Button is not engaged. Plug in the battery.

**Step 3**

Press the Precharge button to precharge the capacitors. Wait 10 seconds

**Step 4**

Turn ON the main switch

**Step 5**

Disengage the precharge button and the system is ready to use.

**Nota Bene**

If ever there is an issue, turn the red button to the OFF position. After having done so, wait for the capacitors to discharge (about 90 seconds) and disconnect the yellow anti-spark connector. If you wish to turn on the system again, repeat from Step 3.

Connecting to the Jetson
~~~~~~~~~~~~~~~~~~~~~~~~
To connect to the Jetson, first make sure you are connected to the ``spot-iot`` Wi-Fi network, then run:

.. code-block:: bash

   ssh byte@172.21.67.198

The password is ``woof1234*``.