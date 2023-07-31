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

Issue commands
--------------
Before sending a power level to the thruster, the thruster must be turned on.

* **thruster.PowerOn**

There's no need to set the power level warning this time.

Now set the power level to MAX.

* **thrusterController.SetPowerLevel MAX**

If you switch quickly to the Events tab and scroll down, you can see the ``thruster.PowerLevelSet`` events being displayed once per second (approximately).

+--------------+----------+---------------------------+----------------+-------------------------------------------------+
| Event Time   | Event Id | Event Name                | Event Severity | Event Description                               |
+==============+==========+===========================+================+=================================================+
| 19:56:23.726 | 0xf03    | thruster.PowerLevelSet    | ACTIVITY_HI    | Thruster power level set to 10%                 |
+--------------+----------+---------------------------+----------------+-------------------------------------------------+
| 19:56:24.727 | 0xf03    | thruster.PowerLevelSet    | ACTIVITY_HI    | Thruster power level set to 20%                 |
+--------------+----------+---------------------------+----------------+-------------------------------------------------+
| 19:56:25.727 | 0xf03    | thruster.PowerLevelSet    | ACTIVITY_HI    | Thruster power level set to 30%                 |
+--------------+----------+---------------------------+----------------+-------------------------------------------------+
| 19:56:26.768 | 0xf03    | thruster.PowerLevelSet    | ACTIVITY_HI    | Thruster power level set to 40%                 |
+--------------+----------+---------------------------+----------------+-------------------------------------------------+
| 19:56:27.768 | 0xf03    | thruster.PowerLevelSet    | ACTIVITY_HI    | Thruster power level set to 50%                 |
+--------------+----------+---------------------------+----------------+-------------------------------------------------+
| 19:56:28.768 | 0xf03    | thruster.PowerLevelSet    | ACTIVITY_HI    | Thruster power level set to 60%                 |
+--------------+----------+---------------------------+----------------+-------------------------------------------------+
| 19:56:29.769 | 0xf03    | thruster.PowerLevelSet    | ACTIVITY_HI    | Thruster power level set to 70%                 |
+--------------+----------+---------------------------+----------------+-------------------------------------------------+
| 19:56:30.769 | 0xf03    | thruster.PowerLevelSet    | ACTIVITY_HI    | Thruster power level set to 80%                 |
+--------------+----------+---------------------------+----------------+-------------------------------------------------+
| 19:56:31.769 | 0xf03    | thruster.PowerLevelSet    | ACTIVITY_HI    | Thruster power level set to 90%                 |
+--------------+----------+---------------------------+----------------+-------------------------------------------------+
| 19:56:32.770 | 0xf03    | thruster.PowerLevelSet    | ACTIVITY_HI    | Thruster power level set to 100%                |
+--------------+----------+---------------------------+----------------+-------------------------------------------------+

Set the power level back to OFF and observe that the gradual power level change also goes downwar.

Conclusion
----------
You have successfully attached the ThrusterComponent to a 1Hz rate group,
and used that to perform a gradual change of the Thruster's power level.
