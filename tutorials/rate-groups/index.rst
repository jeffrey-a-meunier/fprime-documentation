Tutorial 7: Rate groups
=======================

In this tutorial you'll modify the TelemetryController so that when it changes the Thruster's power level,
it does it in increments of 10% once per second.
This allows us to change the power level of the thruster gradually from the existing level to the target leve.

Changing the power level from OFF (0%) to MED (50%) would show a sequence of events like this:

+--------------+------------------------+----------------+---------------------------------+
| Event Time   | Event Name             | Event Severity | Event Description               |
+==============+========================+================+=================================+
| 16:17:53.750 | thruster.PowerLevelSet | ACTIVITY_HI    | Thruster power level set to 10% |
+--------------+------------------------+----------------+---------------------------------+
| 16:17:54.750 | thruster.PowerLevelSet | ACTIVITY_HI    | Thruster power level set to 20% |
+--------------+------------------------+----------------+---------------------------------+
| 16:17:55.751 | thruster.PowerLevelSet | ACTIVITY_HI    | Thruster power level set to 30% |
+--------------+------------------------+----------------+---------------------------------+
| 16:17:56.751 | thruster.PowerLevelSet | ACTIVITY_HI    | Thruster power level set to 40% |
+--------------+------------------------+----------------+---------------------------------+
| 16:17:57.751 | thruster.PowerLevelSet | ACTIVITY_HI    | Thruster power level set to 50% |
+--------------+------------------------+----------------+---------------------------------+

In order to do this, we'll use a *rate group*.
By connecting the ThrusterController to a 1Hz (1 cycle per second) rate group, we can have the rate group call a function in our component once per second.
This function will increment or decrement the thrust level by 10% until it reaches the target value.

.. toctree::
   :maxdepth: 1
   :caption: Contents:
   :numbered:

   existing-project
   add-rate-group-port
   edit-commands
   edit-run-handler
   edit-topology
   build-deployment
   run-gds
