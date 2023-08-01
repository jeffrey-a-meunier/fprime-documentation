Tutorial 8: Telemetry
=====================

A running F' deployment has the ability to send a specific set of data values back to the GDS at regular intervals.
These differ from events, which are specific one-off messages that are sent when something specific happens.

There are four values in our project that we should send to the GDS as telemetry:

In the Thruster component:

* The power status: ``Fw::On _powerStatus``
* The power level percent: ``U8 _powerLevelPercent``

In the ThrusterController component:

* Current power level percent: ``U8 m_currentPercent``.
* Target power level percent ``U8 m_targetPercent``.

In our simulated spacecraft, the two variables ``_powerLevelPercent`` in Thruster and ``m_currentPercent`` in ThrusterController will always be the same, modulo a bit of thread timing jitter.
On a real spacecraft the Thruster ``_powerLevelPercent`` would contain the thrust value obtained from an actual computer that controls an actual thruster,
so it would be crucial for the GDS to display this telemetry value at all times.
The ground controllers need to know if the actual thrust level matches the intended thrust level.
This is the purpose of telemetry.

There are two other values that we might be interested in, namely the ``ShowPowerLevelHiWarn`` and ``PowerLevelHiWarnPercent`` parameters,
but these values already come from the GDS so we won't include them in the telemetry.

We'll use the rate group handler function in both components to send the telemetry back to the GDS.

.. toctree::
   :maxdepth: 1
   :caption: Contents:
   :numbered:

   add-thruster-telemetry
   add-thruster-controller-telemetry
   send-thruster-controller-telemetry
   add-telemetry-channel-packet
   run-gds
