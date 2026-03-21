Software
========

To facilitate communication between the Jetson/PC and the motors, a USB-to-CAN adapter is required. For this project, we are using the following adapter:

.. image:: /assets/byte_research/usb-can-a-1_1.jpg
   :width: 500px
   :align: center
   :alt: USB to CAN Adapter

----

Motor Calibration
-----------------

The motors come pre-flashed from the factory. Instead of flashing, you must perform a calibration sequence using the ODrive tools.

**Setup Requirements:**

* **Python:** Ensure Python is installed on your system.
* **ODrive Tool:** Install via pip:
    .. code-block:: bash

       pip install odrive
* **Zadig:** Download Zadig (included in the `motor documentation zip <https://steadywin-motor.com/products/document-download>`_).

**Driver Configuration (Windows):**

1. Open **Zadig** and select :menuselection:`Options --> List All Devices`.
2. In the main dropdown, select **CyberBeast Motor Driver Device (Interface 2)**.
3. Set the target driver to **WinUSB** (or ``libusb-win32``).
4. Click **Install Driver** (or **Replace Driver**).

**Calibration Procedure:**

Open your terminal or Command Prompt, activate your environment, and run ``odrivetool``. Execute the following commands:

.. code-block:: python

   # Measures internal resistance (Motor will beep)
   odrv0.axis0.requested_state = 4 

   # Position calibration (Motor will turn forward and back)
   odrv0.axis0.requested_state = 7 

   # Save configuration (One-time setup for secondary encoders)
   odrv0.axis0.motor.config.pre_calibrated = 1
   odrv0.axis0.encoder.config.pre_calibrated = 1
   odrv0.save_configuration()

**Testing the Connection:**

To verify the motor responds to commands over USB (Type-C), run:

.. code-block:: python

   odrv0.axis0.requested_state = 8               # Energize motor
   odrv0.axis0.controller.config.control_mode = 2 # Velocity control
   odrv0.axis0.controller.config.input_mode = 1
   odrv0.axis0.controller.input_vel = 10          # Set target speed

----

CAN Communication Prerequisites
-------------------------------

To communicate via CAN using Python, install the necessary libraries:

.. code-block:: bash

    pip install python-can[gs_usb]
    # If using Conda: conda install libusb
    pip install libusb 

**Testing Scripts:**

For validation, refer to the ``CAN_test_sender.py`` and ``CAN_test_receiver.py`` scripts in the repository. These tests typically require two CAN adapters to verify cross-communication.

.. note::
   View the source files in the `EPFL BARK Git Repo <https://github.com/EPFL-AI-Team/BARK/tree/Alex-Branch>`_.