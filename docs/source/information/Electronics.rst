Electronics
===========

PDBueno (Power Distribution Board)
----------------------------------

**The goal of this board is to interface with 3 GIM8108-8 motors for the TEST SETUP**

.. image:: /assets/PCBueno.jpeg
   :width: 800px

|

Requirements for the board are as follows
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 * Power 3x GIM8108-8 motors with GDS68 Driver with 2E68
 * Power a Jetson Orin Nano Super Developer Kit

    * A buck converter such as the `Matek 12S Pro BEC`_ should be present on the board for this. Include a TVS diode to protect from transient voltage spikes

.. _Matek 12S Pro BEC: https://www.lacameraembarquee.fr/pdb-bec-led-fpv/14460-bec-matek-12s-pro-9-55v-vers-5812v-5a.html

 * Wire in series 2x6S LiPo batteries

    * This is to obtain 44.4V and fall withing acceptable voltage to power the motors

 * Include Test Points to be able to measure the volatages in the battery

    * Test points shall be spaced far enough to each other

 * The board shall have 2 layers

Routing Considerations
~~~~~~~~~~~~~~~~~~~~~~

 * The motors have a peak current of 25 A. Therefore according to `IPC-2152`_, a copper thickness of 2oz with a 37 mm trace width should be use (considering the standard 10 degrees rise). Prefer a copper pour to traces.

.. _IPC-2152: https://designertools.app.protoexpress.com/?appid=TWCCCAL&data=ONsTAS5bIR5nFHqIuoQIEqpeYI6FNtC1kNPK9gKk14INuL5nZePBvOxvbHPnk5%20cnHE%20LdwaQf3NPMrsZsHzKD%20%2FM7rjI1AL7TaRuIf%20k%2FaLF5w5yX8NdWEca2p4jr%2Fqd%2F3jiCq2vt109mcdP9itFqb25bcJfU80I08FAEkspybHJoOJbTfc%2FCoK4ZEe1vozrLfjq9Dfs2X42lXYJUnGyJi%20eJFcIvDOZINfUJtZF81UttQPqGh3kPEVQTVWItcA67vr3TNP%20WeEN22%20bXT7AER4Z2z2hsmst3QN4FxVwxvTJQoSn0PzSIu7GDixipPk0cwhm3SBk3Vu9L4HLMaXYQ%3D%3D&q=Mon%20Mar%2009%2013:29:49%20PDT%202026
 
 * The battery shall be connected to the board using 14 AWG wire. 14 AWG is rated for 32 A continuous current and can handle a peak of 75 A for a few seconds. The wire should be kept short.
 * The battery shall be connected with an XT90 connector
 * The motors shall be wired with a 16 AWG wire. 16 AWG wire is rated for 18A which is well over the continuous 7A current of the motors
 * The motors shall be connected to the PCBueno with an XT30 connector

*The final version of the board:*

Front view:

.. image:: /assets/byte_research/PDBueno_front.png
   :width: 500px
   :align: center

|

Back view:

.. image:: /assets/byte_research/PDBueno_back.png
   :width: 500px
   :align: center

|