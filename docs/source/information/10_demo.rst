Demos
=====

Promotional Video Aug 2026
--------------------------

.. video:: /assets/promotional_vid_aug_2026.mp4
   :width: 500

We have 4 demos:

1. **DINGO**: Simple open source quadruped from Stanford
2. **Pupper**: More complex quadruped used in a Stanford class. Can do computer vision and LLM
3. **FootBYTE**: One of BYTE's legs that kicks a football into the FinNET
4. **BYTE**: Our custom quadruped robot

**Please start the robots on their test stands!**

DINGO
-----

By default, on startup the line to launch the robot is run:

.. code-block:: bash

   roslaunch dingo dingo.launch is_physical:=1 is_sim:=0 use_joystick:=1 use_keyboard:=0

Use the **black** PS5 controller. Turn it on and it pairs automatically. The controls are as follows:

.. image:: /assets/dingo/Dingo_Control.jpg
   :width: 500px

|

DINGO has a really hard time walking due to remaining mechanical issues:

.. video:: /assets/dingo/final_dingo.mp4
   :width: 500

Pupper
------

By default, on startup Pupper will calibrate. Once the legs are down, he can be placed on the ground.

Use the **white** PS5 controller to control him. The controls are as follows:

.. image:: /assets/pupper_control.png
   :width: 500px

|

Liam (a legend of the past) was able to train his own policy and give instructions to the robot orally. The dog also used its camera to find objects.
However, this is not shown in the following demo:

.. video:: /assets/final_pupper.mp4
   :width: 500

FootBYTE
--------

One of BYTE's legs on its test stand, kicking a football into the FinNET. The full setup is explained on the :doc:`FootBYTE` page.

Use the **Logitech F710** gamepad, with the slider on the back set to **D**. Hold **RB** to move the leg: left stick for the hip abduct,
right stick for the hip pitch, X and B for the knee.

.. figure:: /assets/byte_research/logitech_f710.jpg
   :width: 500px

   Logitech F710. Photo by FreeMediaKid!, CC BY 4.0, via `Wikimedia Commons <https://commons.wikimedia.org/wiki/File:Logitech_F710,_forward.jpg>`__.

.. video:: /assets/byte_research/footbyte_demo.mp4
   :width: 500

BYTE
----

At the moment, each motor has to be activated one at a time using ODrive.

.. video:: /assets/byte_research/byte_standing_demo.mp4
   :width: 500
