Add telemetry channel packet
============================

Create a new ``Thruster`` packet in ``SpacecraftDeploymentPackets.xml``.
You can put it after the end of the ``SystemRes3`` packet and before the ``<!-- Ignored packets -->`` comment.

.. code-block:: xml

    <packet name="Thruster" id="10" level="3">
        <channel name = "thruster.PowerStatus"/>
        <channel name = "thruster.PowerLevelPercent"/>
        <channel name = "thrusterController.PowerLevelTargetPercent"/>
        <channel name = "thrusterController.PowerLevelPercent"/>
    </packet>

Build the deployment
--------------------
Execute this command in the ``SpacecraftDeployment`` directory.

.. code-block:: bash

    fprime-util build -j4
