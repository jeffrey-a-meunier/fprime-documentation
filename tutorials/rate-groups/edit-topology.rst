Edit topology
=============

All that's left now is to connect the ThrusterComponent to the rate group.
This is what will cause the ``run_handler`` function to get called once per second.

Add it to the rate group
------------------------
Edit the ``SpacecraftDeployment/Top/topology.fpp`` file.

Scroll to the bottom to find the ``connections SpacecraftDeployment`` section,
and add a connection from the rate group to the thruster controller.

.. code-block:: text

    connections SpacecraftDeployment {
      # Add here connections to user-defined components
      thrusterController.PowerLevelCommand -> thruster.SetPowerLevel
      rateGroup1.RateGroupMemberOut[3] -> thrusterController.run
    }

This means that the 4th entry in ``raterGroup1`` (starting counting from 0) will be the ``thrusterController.run`` input port.
If you look in the ``ThrusterController.fpp`` file you can see that input port:

.. code-block:: text

    async input port run: Svc.Sched

That port is called ``run`` and it's type is ``Svc.Sched``.

If you scroll up higer in that file you will see this section:

.. code-block:: text

        connections RateGroups {
      # Block driver
      blockDrv.CycleOut -> rateGroupDriver.CycleIn

      # Rate group 1
      rateGroupDriver.CycleOut[Ports_RateGroups.rateGroup1] -> rateGroup1.CycleIn
      rateGroup1.RateGroupMemberOut[0] -> tlmSend.Run
      rateGroup1.RateGroupMemberOut[1] -> fileDownlink.Run
      rateGroup1.RateGroupMemberOut[2] -> systemResources.run

The final entry so far in ``rateGroup1`` is ``rateGroup1.RateGroupMemberOut[2]``,
so this is why I chose to use ``rateGroup1.RateGroupMemberOut[3]`` in the
``connections SpacecraftDeployment`` section.
