Add ThrusterController telemetry
================================

Here you'll create telemetry channels in the ThrusterController component for the data values we want to send back to the GDS.

Add telemetry channels to the Thruster
--------------------------------------
Edit the ``ThrusterController.fpp`` file and replace the example telemetry channel:

.. code-block:: text

    # @ Example telemetry counter
    # telemetry ExampleCounter: U64

with these two channels:

.. code-block:: text

    @ Target power level percent
    telemetry PowerLevelTargetPercent: U8

    @ Actual power level percent
    telemetry PowerLevelPercent: U8

Unlike the Thruster component, the ``run`` port in the ThrusterController component has already been uncommented.

Run the ``impl`` command to generate the telemetry functions in C++.

.. code-block:: bash

    fprime-util impl
