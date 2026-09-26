FootBYTE
========

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

Motor Control
~~~~~~~~~~~~~
The teleop script runs on the Jetson and drives the three motors of one leg with a gamepad.
It talks to the motors directly over the USB-to-CAN adapter, without ROS.

The three motors share a single CAN bus. Each motor has its own node ID, and every command is a short frame addressed
to one node, so all three are driven over the same two wires. The bus is a twisted pair (CANH and CANL) with a
120 Ohm resistor at each end.

.. figure:: /assets/byte_research/interface-can-bus.png
   :width: 500px

   A CAN bus: every node is connected to the same pair of wires, terminated at both ends.

**What you need:**

* The USB-to-CAN adapter, on ``/dev/ttyUSB0``
* A Logitech F710 gamepad, with the slider on the back set to **D** (DirectInput)

.. figure:: /assets/byte_research/usb-can-a-1_1.jpg
   :width: 400px

   The USB-to-CAN adapter.

.. figure:: /assets/byte_research/logitech_f710.jpg
   :width: 400px

   Logitech F710. Photo by FreeMediaKid!, CC BY 4.0, via `Wikimedia Commons <https://commons.wikimedia.org/wiki/File:Logitech_F710,_forward.jpg>`__.

**The three motors:**

* Left stick, left and right: hip abduct (node 3)
* Right stick, up and down: hip pitch (node 5)
* X and B buttons: knee (node 1)

**RB is a deadman switch.** The leg only moves while RB is held. Release it and the leg holds its position, it does not
move back. The joints are limited to +/-20 deg for the hip abduct, 45 deg back and 20 deg forward for the hip pitch,
and +/-90 deg for the knee.

**Startup:**

1. Every motor is set to IDLE, whatever the previous run left behind.
2. The gamepad is opened. If the receiver is not plugged in, the script stops here and the motors stay IDLE.
   This is why powering the Jetson alone can never energize the leg.
3. Hold RB for 1 second to energize the motors.
4. The script reads the current position of each motor and locks it there. This position is the home of the session.

The position is read after the motors are energized, because they only report their real position once powered.
The targets then move gradually, so the leg does not jump when you arm it.

**Stopping:**

* **Start** puts the motors back in IDLE but keeps the script running. Hold RB again to re-arm, the home position is
  read again since the leg may have been moved by hand.
* **Ctrl-C** sends the leg back to home, then sends IDLE three times to each motor, with a pause between motors.
  Node 5 sometimes ignores a fast burst when the bus is busy.

Demo
~~~~
The leg kicking a football:

.. video:: /assets/byte_research/footbyte_demo.mp4
   :width: 500

Future Works
~~~~~~~~~~~~
I'm not sure what I did, but I managed to configure the motors in a way where they work on the FootBYTE. When I tried
to replicate it on another set of 3 motors, the CAN communication was failing. I tried to put the same config and it
did not work.

We probably need to read the documentation properly, which is available `here <https://steadywin.cn/en/col.jsp?id=124>`_,
and create a test jig to safely test the 3 motors.
