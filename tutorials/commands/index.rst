Tutorial 3: Commands
====================

In F', a *command* is issued by the GDS (ground data system) and is handled by a specific component in a running deployment.

In this tutorial you'll add three new commands to the ``Thruster`` component in order to turn the thruster on or off, and to set its power level.

The component will accept commands but it will not report any information back to the GDS.
Sending information from the component to the GDS requires the use of *events*, which will be presented in the next tutorial.

In this tutorial you'll need to use a text editor to edit ``.fpp`` and ``.cpp`` files.

.. toctree::
   :maxdepth: 1
   :caption: Contents:
   :numbered:

   existing-project
   add-commands
   build-deployment
   run-gds
   summary
