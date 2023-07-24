Add events
==========
In this section you will add new events to the ``Thruster`` component.
We'll use these events to report information back to the GDS when each of the commands is invoked.

Replace the example event
-------------------------
Edit the ``Thruster.fpp`` file in the ``Components/Thruster`` directory.

Locate the existing ``ExampleStateEvent`` event:

.. code-block:: text

    # @ Example event
    # event ExampleStateEvent(example_state: Fw.On) severity activity high id 0 format "State set to {}"

Replace it with these events:

.. code-block:: text

    event PowerOn() \
        severity activity high \
        format "INFO: Thruster power ON"

    event PowerOff() \
        severity activity high \
        format "INFO: Thruster power OFF"

    event PowerStatus(onOff: Fw.On) \
        severity activity high \
        format "Power is {}"

    event PowerLevelSet(percent: U8) \
        severity activity high \
        format "Thruster power level set to {}%"

    event LevelSetPowerOffWarning() \
        severity warning low \
        format "Thruster power must be turned on before setting level"

Note the use of the ``Fw.On`` type in the ``PowerStatus`` event.
``Fw`` means *framework*, and there are a number of types and data structures defined in the F' ``Fw`` namespace.
The ``Fw.On`` class is similar to the ``bool`` type in C++, but it is designed specifically to work with the underlying F' system.
(In particular, it extends the F' ``Fw::Serializable`` class.)

Furthermore, in the context of an embedded system connected to hardware devices, the values ``Fw::On::ON`` and ``Fw::On::OFF`` convey more information to the reader than the simple constants ``true`` and ``false``.

Generate the implemetation
--------------------------
In the ``Components/Thruster`` directory, enter this command:

.. code-block:: bash

    fprime-util impl

This command is required each time you edit the ``.fpp`` file, but it doesn't always generate new code in the ``.cpp-template`` and ``.hpp-template`` files, and that's the case this time.
That means that we don't need to do anything with the newly generated template files -- they're the same as the previous template files.

Event function call format
--------------------------
We created events with two different severity levels: ``activity high`` and ``warning low``.

.. code-block:: text

    event PowerOn() severity activity high
    event PowerOff() severity activity high
    event PowerStatus(onOff: Fw.On) severity activity high
    event PowerLevelSet(percent: U8) severity activity high
    event LevelSetPowerOffWarning() severity warning low

The severity level becomes part of the event function name when it's converted to C++, and that must be specified when calling any of the functions:

.. code-block:: c++

    log_ACTIVITY_HI_PowerOn();
    log_ACTIVITY_HI_PowerOff();
    log_ACTIVITY_HI_PowerStatus(Fw::On onOff);
    log_ACTIVITY_HI_PowerLevelSet(U8 percent);
    log_WARNING_LO_LevelSetPowerOffWarning();

This discinction allows events to be defined that have the same name but differ only in their severity levels.
