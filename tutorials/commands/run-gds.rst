Run GDS
=======

Be sure you're in the ``SpacecraftDeployment`` directory.

Execute one of these two commands, depending on your development environment (see the previous tutorial for details).

Either:

.. code-block:: bash

    fprime-gds

or:

.. code-block:: bash

    fprime-gds --gui-addr 0.0.0.0

Several log info messages are displayed, with the last line saying ``F prime is now running.``

.. code-block:: text

    [INFO] F prime is now running. CTRL-C to shutdown all components.

Open a web browser
------------------
If you're working natively or locally, a web browser window should have opened automatically.
Otherwise, open a web browser and navigate to http://localhost:5000.

.. image:: images/gds-1.jpg

Notice the green circle icon in the upper right corner: this shows that the flight software deployment has connected to the GDS system.
You may have to wait a few seconds for the red X to change into a green circle.
If the red X does not change into a green circle, then navigate to the **Log** tab and look for error messages.

Using the GDS
-------------
Click the **Mnemonic** drop-down box to see all the commands that are provided by the components in your flight software deployment.
Locate and select the command **thruster.SetPowerLevel**.

Recall that ``thruster`` is the name we gave to the instance, and ``SetPowerLevel`` is the name of the command that was defined in the ``.fpp`` file.

.. image:: images/thruster-setpowerlevel.jpg

Type the number **50** in the **percent** text box, then press the orange **Send Command** button to send the command to the thruster.

The command log at the bottom of the window should indicate that the command was sent.
Your **Command Time** will be different from mine, but the **Command String** should be the same.

.. image:: images/command-log.jpg

Do the same for the two commands **thruster.PowerOn** and then **thruster.PowerOff**.

Observe the entries in the command log on the bottom of the window.

Then switch to the **Events** tab to view the complete list of events associated with the three commands that were sent to the spacecraft thruster.

Conclusion
----------
You have completed implementing and testing three new commands in the Thruster component.

You can press ^C (Ctrl-C) in the ``fprime-gds`` terminal window to terminate the GDS server.
