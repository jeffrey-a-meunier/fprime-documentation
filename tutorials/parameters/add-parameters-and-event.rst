Add parameters and an event
===========================

In this section you will add new parameters to the ``Thruster`` component.
We'll use these parameters to determine if a power level warning should be issued.
A new event will be used when the warning is issued.

The first parameter will indicate whether or not the warning message will be shown.
We have several options for the type of this parameter.
On the one hand we could use just a simple ``bool`` value, but on the other hand F' has a few enumerated types that convey a little more information than just ``true`` or ``false``.

If you look at the file ``fprime/Fw/Types/Types.fpp`` you will see these two enumerated types:

.. code-block:: text

  @ Enabled and disabled states
  enum Enabled {
    DISABLED @< Disabled state
    ENABLED @< Enabled state
  }

  @ On and off states
  enum On {
    OFF @< Off state
    ON @< On state
  }

I think the words ``ENABLED`` and ``DISABLED`` best describe the state of the warning message, so I'll choose to use the ``Enabled`` enumerated type.

Replace the example parameter
-----------------------------
Edit the ``Thruster.fpp`` file in the ``Components/Thruster`` directory.

Locate the existing ``PARAMETER_NAME`` evparameter:

.. code-block:: text

    # @ Example parameter
    # param PARAMETER_NAME: U32

Replace it with these parameters:

.. code-block:: text

    @ Parameter to enable or disable the power level warning
    param ShowPowerLevelHiWarn: Fw.Enabled default Fw.Enabled.DISABLED

    @ Parameter to indicate the level at which to issue the power level warning
    param PowerLevelHiWarnPercent: U8 default 100

Notice that the parameters have default values: the default value of ``ShowPowerLevelHiWarn`` is ``OFF``, and the default value of ``PowerLevelHiWarnPercent`` is 100.
These default values are used if the parameter value is unable to be retrieved.
That means that even if it were switched on, the power level warning wouldn't be shown unless the power level exceeded 100%, which it should never do.

Add a new event
---------------
Add this new event to the ``Thruster.fpp`` file.
This is the event that will be issued to the GDS if we detect that the power level is set to a value above the hi-warn limit.

.. code-block:: text

    event PowerLevelHiWarn(limit: U8, level: U8) \
        severity warning high \
        format "Thruster level {}% is above the hi-warn limit {}%"

Generate the implemetation
--------------------------
In the ``Components/Thruster`` directory, enter this command:

.. code-block:: bash

    fprime-util impl

This command is required each time you edit the ``.fpp`` file, but it doesn't always generate new code in the ``.cpp-template`` and ``.hpp-template`` files, and that's the case this time.
That means that we don't need to do anything with the newly generated template files -- they're the same as the previous template files.

Build the component
-------------------

.. code-block:: bash

    fprime-util build

This will generate a number of auto-coded functions that will enable you to read the parameter values.
