Write to the output port
========================

Here you'll modify the ThrusterController's ``SetPowerLevel`` command so that it receives the ``PowerLevelPreset`` value,
and then sends the appropriate power level to the ``Thruster`` component over the port.

Recall the table of presets:

+------+-------+
| Name | Value |
+======+=======+
| OFF  | 0%    |
+------+-------+
| LOW  | 25%   |
+------+-------+
| MED  | 50%   |
+------+-------+
| HI   | 75%   |
+------+-------+
| MAX  | 100%  |
+------+-------+

The simplest way to implement this table in C++ is to create an array of Values, and then the Name enumeration symbol can be used as the index into that array.
I'll explain how to do that below.

Observe the type of the ``level`` parameter of the ``SetPowerLevel_cmdHandler`` function:

.. code-block:: c++

    Components::ThrusterController_PowerLevelPreset level

This is the implementation type of the ``PowerLevelPreset`` enum that you defined in the ``.fpp`` file.
Normally you'd use the enum value as-is, but here it will be necessary to obtain an integer value associated with the enum value.

It turns out that each enum value already has an integer associated with it. Thus, this:

.. code-block:: c++

    enum PowerLevelPreset {
        OFF,
        LO,
        MED,
        HI,
        MAX
    }

is the same as this:

.. code-block:: c++

    enum PowerLevelPreset {
        OFF = 0,
        LO  = 1,
        MED = 2,
        HI  = 3,
        MAX = 4
    }

and the way to obtain the integer associated with any of the enum values is to use the ``.e`` field discriminator.

Also, now that the output port has been defined in the ``.fpp`` file,
this is the function in the ``ThrusterControllerComponentAc.cpp`` autocode file that lets you send data over the ``PowerLevelCommand`` output port.

.. code-block:: c++

    //! Invoke output port PowerLevelCommand
    //!
    void PowerLevelCommand_out(
        NATIVE_INT_TYPE portNum, /*!< The port number*/
        U8 percent 
    );

Modify ThrusterController's ``SetPowerLevel_cmdHandler``
--------------------------------------------------------
Edit the ``ThrusterController.cpp`` file and find the ``SetPowerLevel_cmdHandler`` function.
Add these statements before the ``this->cmdResponse_out`` statetment.

First define the array of presets:

.. code-block:: c++

    U8 presetPercents[] = {0, 25, 50, 75, 100};

Then choose the preset indicated by the ``level`` parameter:

.. code-block:: c++

    U8 presetPercent = presetPercents[level.e];

Then send the percent value out the port:

.. code-block:: c++

    PowerLevelCommand_out(0, presetPercent);

(C++ nerd stuff)
~~~~~~~~~~~~~~~~

The way the ``presetPercents`` array is defined right now, it is reallocated each time the ``SetPowerLevel_cmdHandler`` function is called,
but it would be better to create the array only one time. There are two ways to achieve that:

#. Make the ``presetPercents`` a member variable: This is not the best solution, because ``presetPercents`` is needed only by this function, and it's always best to limit the scope of a variable as much as possible.
#. Add ``static const`` to the variable type: This is better because it leaves the array encapsulated inside the only function that needs it.
   Making it ``static`` means that the array will be created only one time (``static`` local variables retain their values between function calls),
   and making it ``const`` tells the compiler that we never intend to change the value of the ``presetPercents`` variable.

Thus, if you want to make this change, then the statement should look like this:

.. code-block:: c++

    static const U8 presetPercents[] = {0, 25, 50, 75, 100};

In fact, in any C++ program you write, if you know that the value of a variable will not change, then it's a good idea to make that variable ``const``.

For example, after defining the ``presetPercents`` array we create the ``U8 presetPercent`` variable.
The variable is used only one time and its value never changes, so it would be better to add ``const`` to that definition:

.. code-block:: c++

    const U8 presetPercent = presetPercents[level.e];

Build it
--------

.. code-block:: bash

    fprime-util build
