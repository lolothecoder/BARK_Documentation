Mechanics
=========

Leg Concept
-----------

The legs should have three degrees of freedom. There are a few ideas to explore for the legs:

1. Three motors close to the base + rigid link for transmission
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* Solid configuration with motors close to the body of the dog.
* The transmission is OK; it's a rigid link.

.. image:: /assets/byte_research/3motors_with_rigid_link.jpeg
   :width: 500px
   :align: center

|

2. Three motors close to the base + Belt for transmission 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* This configuration uses belts for power transfer (Could fall off)
* Motors close to body

.. image:: /assets/byte_research/belt.png
   :width: 500px
   :align: center

|

3. Three motors at the joints
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* This configuration removes the need for a transmission
* However with bulky motors it might be hard to fit them

.. image:: /assets/byte_research/PupperLeg.jpg
   :width: 500px
   :align: center

|

First Iteration
---------------

.. image:: /assets/byte_research/Leg.jpeg
   :width: 500px
   :align: center

|

.. video:: /assets/byte_research/3dof.mp4
  :width: 80%
  :align: center

|

Test Setup
----------

The goal of the test setup is to be able to test the 3 DOF of a leg

 * It should support the leg and be stable
 * It should measure one meter tall
 * It should allow the leg to freely move 
 * We should be able to to add weights to measure th eload on one leg
 * It should provide a space to place the PDBueno and the battery

.. image:: /assets/byte_research/test_setup.png
   :width: 500px
   :align: center

|

Update of the test setup by Killian:

Updated @ChristyElSkaff 's design to use 2020 v-slot profiles and set the height of the rig to 1m high following the stand up meeting discussion, next we will need to update the top plate design to fit all the electronics and the weights as well as design the attachement plate that will hold the leg once the leg team will have an approximate design.

The rigidity of the rig may be an issue due to the height, in which case we will do a secondary arch to make it into a box design, we should have the necessary materials to do so with the list of parts I sent to @lolothecoder today.

.. image:: /assets/byte_research/testv2.png
   :width: 500px
   :align: center

|

BOM for the Test Setup:

.. image:: /assets/byte_research/test_setup_bom.png
   :width: 800px
   :align: center

|

