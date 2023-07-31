ThrusterController PowerLevelSet event
======================================

I created this event in ThrusterController.fpp

.. code-block:: text

    event PowerLevelSet(level: PowerLevelPreset) \
        severity activity high \
        format "ThrusterController power level set to {}%"

But I don't think I use it anywhere. I may not have even put it into the tutorial.

---

Confirmed, I never put it in the tutorial, and it's not in the code.

Instead of using this event, describe how the event log in GDS gives you the same information.

* The command starts.
* The command gets sent.
* The command completes successfully.
  * This means that the ``OK`` value was sent back from the command handler.
