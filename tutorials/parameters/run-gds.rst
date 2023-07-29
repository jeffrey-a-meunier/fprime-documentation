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

* **thruster.PowerLevelHiWarnPercent_PRM_SAVE**
* **thruster.PowerLevelHiWarnPercent_PRM_SET**
* **thruster.ShowPowerLevelHiWarn_PRM_SAVE**
* **thruster.ShowPowerLevelHiWarn_PRM_SET**

The **PRM_SET** commands let you set the value of the parameter in the component.

**[VERIFY THIS]** The **PRM_SAVE** commands cause the parameter values to be stored in non-volatile memory in the spacecraft.
If the spacecraft computer were to reboot, it would use the last saved parameter value.

Issue commands
--------------
Here we'll verify that the two parameters work the way we want them to.

Issue these commands to the component in this order:

* **thruster.PowerOn**
* **thruster.SetPowerLevel, 95**
* Check the Events tab: you should see two **ACTIVITY_HI** events indicating that the thruster power is ON and set to 95%.
* **thruster.PowerLevelHiWarnPercent_PRM_SET, 90**
* Check Events: you should see two new **COMMAND** events, the first is **thruster.PowerLevelHiWarnPercent_PRM_SET dispatched to port 8** and the next one is similar but it says **completed**.
* **thruster.SetPowerLevel, 95**
* Check Events: there should be no power level warning since the warning hasn't been enabled.
* **thruster.ShowPowerLevelHiWarn_PRM_SET, ON**
* **thruster.SetPowerLevel, 95**
* Check Events: there should be a **WARNING_HI** event that says **Thruster level 95% is above the warn limit 90%**.

Conclusion
----------
You have added several parameters to your component that let the GDS affect how the component runs.

You can press ^C (Ctrl-C) in the ``fprime-gds`` terminal window to terminate the GDS server.
