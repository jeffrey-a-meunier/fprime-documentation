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

Issue commands
--------------
Click the **Mnemonic** drop-down box to see all the commands that are provided by the components in your flight software deployment.
You should see these commands at the bottom of the list:

* **thruster.PowerOn**
* **thruster.PowerOff**
* **thruster.PowerStatus**
* **thruster.SetPowerLevel**

Check the power status
~~~~~~~~~~~~~~~~~~~~~~
Send the command **thruster.PowerStatus**, then check the Events tab.
You should see these evnts listed:

======================== ============== =========================================
Event Name               Event Severity Event Description
======================== ============== =========================================
cmdDisp.OpCodeDispatched COMMAND        thruster.PowerStatus dispatched to port 8
thruster.PowerStatus     ACTIVITY_HI    Power is OFF
cmdDisp.OpCodeCompleted  COMMAND        thruster.PowerStatus completed
======================== ============== =========================================

In particular, we're interested in the **thruster.PowerStatus** event that says **Power is OFF**.
As it should be.

Notice that the **COMMAND** events all have a green background and the **ACTIVITY_HI** command has a blue background.

Set the power level
~~~~~~~~~~~~~~~~~~~
Back in the **Commanding** tab, select the **thruster.SetPowerLevel** command, and enter **50** in the **percent** argument text box.
Press the **Send Command** button and then view the Events again.

================================ ============== =========================================
Event Name                       Event Severity Event Description
================================ ============== =========================================
cmdDisp.OpCodeDispatched         COMMAND        thruster.SetPowerLevel dispatched to port 8
thruster.LevelSetPowerOffWarning WARNING_LO     Thruster power must be turned on before setting level.
cmdDisp.OpCodeCompleted          COMMAND        thruster.SetPowerLevel completed
================================ ============== =========================================

This time we're looking for the **WARNING_LO** event **Thruster power must be turned on before setting level.**
This event has a yellow background.

Power on, set level, power off
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Execute these three commands in order, and observe the events after each one.

#. **thruster.PowerOn**: Look for event description **Thruster power ON**.
#. **thruster.PowerStatus**: Look for event description **Power is ON**.
#. **thruster.SetPowerLevel 75**: Look for event description **Thruster power level set to 75%**.
#. **thruster.PowerOff**: Look for event description **Thruster power OFF**.

Conclusion
----------
You have added several events to your component, and used them to communicate from the component to the GDS.

You can press ^C (Ctrl-C) in the ``fprime-gds`` terminal window to terminate the GDS server.
