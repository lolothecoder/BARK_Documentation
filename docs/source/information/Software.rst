Software
========

In order to be able to communicate between the Jetson/PC to the motors. We require a USB to CAN adapter. This is the one we chose:

.. image:: /assets/byte_research/usb-can-a-1_1.jpg
   :width: 500px

|

Flashing Motors
~~~~~~~~~~~~~~~
All the flashing documentation for the GDS68 Driver is given under this link : https://steadywin-motor.com/products/document-download (in chinese)

Download the required firmware files from the site and use “Nations MCU Download Tool” through USB. Follow the documentation steps in the pdf file of the site for the details.

Prerequisites for communication via CAN in python
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: bash

    pip install python-can[gs_usb]
    pip install libusb (if using conda env use conda install)

For testing purposes, create a CAN_test_sender.py and CAN_test_receiver.py to test sending simple messages across the CAN (we used 2 CAN adapters here) 
See files in git repo: https://github.com/EPFL-AI-Team/BARK/tree/Alex-Branch
