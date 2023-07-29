Tutorial: Components
====================
This tutorial is designed to introduce you to *components* in F'.
Each F' project consists of one or more interacting components.

In this tutorial you’ll create a single component.
We're not using any actual hardware devices or spacecraft, so we'll imagine that we have a spacecract that has a single thruster that needs to be controlled from the ground.

The users on the ground can send *commands* to the thruster in order to turn it on and off, and to set its thrust level.

In subsequent tutorials you'll see how the component can send *events* and *telemetry* back to the ground.

.. toctree::
   :maxdepth: 1
   :caption: Contents:
   :numbered:

   existing-project
   create-component
   create-deployment
   modify-topology
   build-deployment
   run-gds
   summary
