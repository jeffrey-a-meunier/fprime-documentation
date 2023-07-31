Add presets, a command, and an event
====================================

In this section you'll add declarations to the ``ThrusterComponent.fpp`` file to allow the GDS to command the ThrusterComponent,
and to report an event to the GDS.

Add presets
-----------
In the ``ThrusterController.fpp`` file add this ``enum``:

.. code-block:: c++

    enum PowerLevelPreset {
        OFF,
        LO,
        MED,
        HI,
        MAX
    }

These represent the five separate presets that we can use to set the Thruster's power level.

Add a new command
-----------------
We will now add a new command to the ThrusterController that lets the GDS set the power level to one of the five presets.

Add this to ``ThrusterController.fpp``:

.. code-block:: text

    @ Command to set the thruster power level
    async command SetPowerLevel(level: PowerLevelPreset)

Add an event
------------
We will add an event that can be sent to the GDS whenever the new ``SetPowerLevel`` command is called.

Add this to ``ThrusterController.fpp``:

.. code-block:: text

    event PowerLevelSet(level: PowerLevelPreset) \
        severity activity high \
        format "ThrusterController power level set to {}%"

Implement and build
-------------------
Enter these commands in the ``ThrusterComponent`` directory:

.. code-block:: bash

    fprime-util impl
    fprime-util build
