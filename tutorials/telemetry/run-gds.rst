Run GDS
=======

Be sure you're in the ``SpacecraftDeployment`` directory.

Execute one of these two commands, depending on your development environment (see the previous tutorial for details).

Either:

.. code-block:: text

    fprime-gds

or:

.. code-block:: text

    fprime-gds --gui-addr 0.0.0.0

Several log info messages are displayed, with the last line saying ``F prime is now running.``

.. code-block:: text

    [INFO] F prime is now running. CTRL-C to shutdown all components.

Open a web browser
------------------
If you're working natively or locally, a web browser window should have opened automatically.
Otherwise, open a web browser and navigate to http://localhost:5000.

Observe telemetry channels
--------------------------
Click the **Channels** tab. You should see the four new telemetry channels:

+--------------------------------------------+---------------+
| Channel Name                               | Channel Value |
+============================================+===============+
| thruster.PowerStatus                       | OFF           |
+--------------------------------------------+---------------+
| thruster.PowerLevelPercent                 |               |
+--------------------------------------------+---------------+
| thrusterController.PowerLevelTargetPercent |               |
+--------------------------------------------+---------------+
| thrusterController.PowerLevelPercent       |               |
+--------------------------------------------+---------------+

Notice that values that are 0 are not shown at all.

Set the power level
-------------------
Send these commands:

* **thruster.PowerOn**

Check the telemetry channels tab again. You should see that the **thruster.PowerStatus** has changed from **OFF** to **ON**.
The power level values are still empty.

This next command will cause the ThrusterController to increase the power level from 0 to 100.
It should take 10 seconds to do so.
This should give you enough time after you enter the command to switch quickly to the **Channels** tab to watch the telemetry values change in real time.

* **thrusterController.SetPowerLevel, MAX**

Send that command and then switch to the **Channels** tab and you should see the thruster telemetry changing.

+--------------------------------------------+---------------+
| Channel Name                               | Channel Value |
+============================================+===============+
| thruster.PowerStatus                       | OFF           |
+--------------------------------------------+---------------+
| thruster.PowerLevelPercent                 | 60            |
+--------------------------------------------+---------------+
| thrusterController.PowerLevelTargetPercent | 100           |
+--------------------------------------------+---------------+
| thrusterController.PowerLevelPercent       | 70            |
+--------------------------------------------+---------------+

Set the power level to **OFF** and you can watch the telemetry change again.

* **thrusterController.SetPowerLevel, OFF**

Conclusion
----------
You have now explored all the main features that F' has to offer.
If you followed along from start to finish, then you have completed tutorials on these topics:

* Components
* Commands
* Events
* Parameters
* Ports
* Rate groups
* Telemetry

What's left now is to learn how to create unit tests for your components.
