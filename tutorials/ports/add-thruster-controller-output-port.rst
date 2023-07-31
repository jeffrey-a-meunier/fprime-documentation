Add ThrusterController output port
==================================

Thankfully, adding an output port is much simpler than adding an input port.
An output port has a single auto-coded function to call, and there's no handler function that you need to fill in.

In the ``ThrusterController.fpp`` file create a new output port.

.. code-block:: text

    output port PowerLevelCommand: PowerLevelPort

Implement and build
-------------------

.. code-block:: bash

    fprime-util impl
    fprime-util build

You should see this:

.. code-block:: text

    [100%] Built target Components_ThrusterController

Conclusion
----------
Here we've given the ThrusterController the ability to send the thruster power level over a port to another component, but the components are not connected yet.
The topology must still be modified in order to connect the ThrusterController's ``PowerLevelCommand`` output port to the Thruster's ``SetPowerLevel`` input port.
