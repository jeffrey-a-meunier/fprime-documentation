Tutorial: Parameters
====================

A **parameter** in F' is a non-volatile global value that can be modified through a command from the GDS.
The main benefit of using a parameter is that the parameter's value can be stored in a database on the spacecraft.
In the event of a system reboot on the spacecraft, the last stored value of the parameter will be retrieved from the database.

In this tutorial you'll add two parameters to the Thruster component that indicates whether a warning event should be issued if the power level is set too high, and what the warning power level is.

.. toctree::
   :maxdepth: 1
   :caption: Contents:
   :numbered:

   existing-project
   add-parameters-and-event
   edit-commands
   build-deployment
   run-gds
