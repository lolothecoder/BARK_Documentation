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
