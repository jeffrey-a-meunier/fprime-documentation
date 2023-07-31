Modify the topology
===================

The way to connect the ThrusterController's output port to the Thruster's input port is by configuring the ports in the project's deployment.

Edit the ``SpacecraftDeployment/Top/topology.fpp`` file and locate the ``connections SpacecraftDeployment`` section,
then add this line to it:

.. code-block:: text

    thrusterController.PowerLevelCommand -> thruster.SetPowerLevel

Now it should look like this:

.. code-block:: text

    connections SpacecraftDeployment {
      # Add here connections to user-defined components
      thrusterController.PowerLevelCommand -> thruster.SetPowerLevel
    }

Remember that ``PowerLevelCommand`` is the name of the ThrusterController's output port,
and ``SetPowerLevel`` is the name of the Thruster's input port.
