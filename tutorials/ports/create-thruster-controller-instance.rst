Create a ThrusterController instance
====================================

Create the instance
-------------------
Add this to the ``SpacecraftDeployment/Top/instances.fpp`` file.
Put it just below the ``Components.Thruster`` instance declaration.

.. code-block:: text

  instance thrusterController: Components.ThrusterController base id 0x1000 \
      queue size Default.QUEUE_SIZE \
      stack size Default.STACK_SIZE \
      priority 50

Ensure that the base id (0x1000) does not conflict with any other base IDs in the topology.

Add it to the topology
-----------------------
Add this to the ``SpacecraftDeployment/Top/topology.fpp`` file.
Put it just below ``instance thruster``.

.. code-block:: text

    instance thrusterController

In the next section you'll continue editing the ``topology.fpp`` file.