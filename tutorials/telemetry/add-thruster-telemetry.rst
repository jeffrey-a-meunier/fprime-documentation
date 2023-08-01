Add Thruster telemetry
======================

Here you'll create telemetry channels in the Thruster component for the data values we want to send back to the GDS.

Add telemetry channels to the Thruster
--------------------------------------
Edit the ``Thruster.fpp`` file and replace the example telemetry channel:

.. code-block:: text

    # @ Example telemetry counter
    # telemetry ExampleCounter: U64

with these two channels:

.. code-block:: text

    @ Target power level percent
    telemetry PowerStatus: Fw.On

    @ Actual power level percent
    telemetry PowerLevelPercent: U8

Also make sure that the ``run`` port is un-commented, and that the port is ``async`` and not ``sync``:

.. code-block:: text

    @ Port to receive calls from the rate group
    async input port run: Svc.Sched

Run the ``impl`` command to generate the telemetry and port functions in C++.

.. code-block:: bash

    fprime-util impl

Send telemetry values
---------------------
Edit the ``Thruster.cpp`` file and locate the ``run_handler`` function.
You can see that it already has an example telemetry function call in it.

.. code-block:: c++

  void Thruster ::
    run_handler(
        const NATIVE_INT_TYPE portNum,
        NATIVE_UINT_TYPE context
    )
  {
    // this->tlmWrite_ExampleCounter(0);
  }

Replace that commented out function call with these function calls:

.. code-block:: c++

    this->tlmWrite_PowerStatus(this->_powerStatus);
    this->tlmWrite_PowerLevelPercent(this->_powerLevelPercent);

Build it to be sure you typed everything correctly.

.. code-block:: bash

    fprime-util build

Add it to the rate group
------------------------
The ``run`` port must be connected to the rate group in order for it to be called once per second, like the ThrusterController component.

Edit the ``SpacecraftDeployment/Top/topology.fpp`` file.

Scroll to the bottom to find the ``connections SpacecraftDeployment`` section,
and add a connection from the rate group to the ``thruster.run`` port.

.. code-block:: text

    connections SpacecraftDeployment {
      # Add here connections to user-defined components
      thrusterController.PowerLevelCommand -> thruster.SetPowerLevel
      rateGroup1.RateGroupMemberOut[3] -> thrusterController.run
      rateGroup1.RateGroupMemberOut[4] -> thruster.run
    }
