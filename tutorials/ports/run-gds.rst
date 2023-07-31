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

Observe new commands
--------------------
After all the work you did in this tutorial, there is only one new command listed in the **Mnemonic** drop-down list, way at the bottom:

* **thrusterController.SetPowerLevel**

Issue commands
--------------
Before sending a power level to the thruster, the thruster must be turned on.

* **thruster.PowerOn**

Let's also set the power level warning to 90%.

* **thruster.PowerLevelHiWarnPercent_PRM_SET, 90**
* **thruster.ShowPowerLevelHiWarn_PRM_SET, ENABLED**

Before testing the **thrusterController.SetPowerLevel** command, set the thruster to a specific power level so that it will be easy to determine if the **thrusterController.SetPowerLevel** command is working.
Use the **thruster.SetPowerLevel** command to do this because it's already been tested.

* **thruster.SetPowerLevel, 15**

Check the **Events** tab. You should see these event descriptions:

* **thruster.PowerOn dispatched to port 8**
* **Thruster power ON**
* **thruster.PowerOn completed**
* **thruster.PowerLevelHiWarnPercent_PRM_SET dispatched to port 8**
* **thruster.PowerLevelHiWarnPercent_PRM_SET completed**
* **thruster.ShowPowerLevelHiWarn_PRM_SET dispatched to port 8**
* **thruster.ShowPowerLevelHiWarn_PRM_SET completed**
* **thruster.SetPowerLevel dispatched to port 8**
* **Thruster power level set to 15%**
* **thruster.SetPowerLevel completed**

The thruster is now running at 15%, so it's time to test the ThrusterController.

Use the ThrusterController to send each power level to the thruster one at a time, from lowest to highest.

* **thrusterController.SetPowerLevel OFF**

There should be three new event descriptions:

* **thrusterController.SetPowerLevel dispatched to port 9**
* **Thruster power level set to 0%**
* **thrusterController.SetPowerLevel completed**

Recall that the **Thruster power level set to 0%** event comes fromt the Thruster component,
so this shows that even though you sent the **SetPowerLevel** command to the ThrusterController component,
the ThrusterController is in fact able to set the Thruster component's power level.

Repeat the **thrusterController.SetPowerLevel** command for the settings **LOW**, **MED**, **HI**, and **MAX**.

Sometimes the **COMMAND** events and the **ACTIVITY_HI** events are not in the order you expect, but you should see a **thruster.PowerLevelSet** command for each command you sent.

+------------------------+-------------+----------------------------------+
| thruster.PowerLevelSet | ACTIVITY_HI | Thruster power level set to 0%   |
+------------------------+-------------+----------------------------------+
| thruster.PowerLevelSet | ACTIVITY_HI | Thruster power level set to 25%  |
+------------------------+-------------+----------------------------------+
| thruster.PowerLevelSet | ACTIVITY_HI | Thruster power level set to 50%  |
+------------------------+-------------+----------------------------------+
| thruster.PowerLevelSet | ACTIVITY_HI | Thruster power level set to 75%  |
+------------------------+-------------+----------------------------------+
| thruster.PowerLevelSet | ACTIVITY_HI | Thruster power level set to 100% |
+------------------------+-------------+----------------------------------+

You should also see an orange warning event:

+---------------------------+------------+-------------------------------------------------+
| thruster.PowerLevelHiWarn | WARNING_HI | Thruster level 100% is above the warn limit 90% |
+---------------------------+------------+-------------------------------------------------+

Conclusion
----------
You have created an input port and an output port, and used a ThrusterController component to communicate with the Thruster component over the port.

You can press ^C (Ctrl-C) in the ``fprime-gds`` terminal window to terminate the GDS server.
