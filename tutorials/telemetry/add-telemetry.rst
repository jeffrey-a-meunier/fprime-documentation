Add telemetry
=============
Add telemetry.

Add telemetry channel
---------------------
Add the channel to the pre-existing MathReceiver packet in ``SpacecraftDeploymentPackets.xml``:

.. code-block:: xml

    <packet name="MathReceiver" id="22" level="3">
        <channel name = "mathReceiver.OPERATION"/>
        <channel name = "mathReceiver.FACTOR"/>
        <channel name = "mathReceiver.NUMBER_OF_OPS"/>  <!-- Add this line -->
    </packet>