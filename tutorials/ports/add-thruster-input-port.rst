Add a Thruster input port
=========================
A port is used to send information from one component to another.
The component that sends information must have an output port defined.
The component that receives information must have an input port defined having the same type as the output port.

After the ports are defined, the topology must be modified to connect the output port to the input port.

Create a Thruster input port type
-------------------------------------
In the Thruster component we'll define a port type and then create an input port.

Edit the ``Thruster.fpp`` file and add this port type definition just inside the module, but before the component definition.

.. code-block:: text

    @ Port for sending and receiving power level percent values
    port PowerLevelPort(percent: U8)

The file should now look like this.

.. code-block:: text

    module Components {

        @ Port for sending and receiving power level percent values
        port PowerLevelPort(percent: U8)

        @ Spacecraft thruster.
        active component Thruster {

We'll need to use this same port type in the ``ThrusterComponent``,
and since ``PowerLevelPort`` is declared inside the ``Components`` module,
it becomes available to all other components defined in that module automatically.

Create a Thruster input port
--------------------------------
In the ``Thruster.fpp`` file create a new input port.

.. code-block:: text

    async input port SetPowerLevel

A port input function will be called each time data arrives on that port.
Later you'll modfy that function to handle the input value.

Run the ``impl`` command to generate the C++ code for that input port.

.. code-block:: bash

    fprime-util impl

This creates an input port handler function that should be copied from the ``.template`` files.

Copy templates
--------------
Copy this function from ``Thruster.hpp-template`` into ``Thruster.hpp``.
Place it below the ``run_handler`` function.

.. code-block:: c++

    //! Handler implementation for SetPowerLevel
    //!
    void SetPowerLevel_handler(
        const NATIVE_INT_TYPE portNum, /*!< The port number*/
        U8 percent 
    );

Copy this function from ``Thruster.cpp-template`` into ``Thruster.cpp``.
Place it below the ``run_handler`` function.

.. code-block:: c++

  void Thruster ::
    SetPowerLevel_handler(
        const NATIVE_INT_TYPE portNum,
        U8 percent
    )
  {
    // TODO
  }

The ``portNum`` parameter is not needed in this function.
It allows the sender to send an integer value as kind of a tag to the second parameter values:
*If the port number is 0, do this with the data. Othwrwise if the port number is 1, then do this other thing...*

The ``percent`` parameter is the power level percent to wihch the thruster should be set.

Notice that we have a command called ``SetPowerLevel`` and an input port called ``SetPowerLevel``,
but these two names don't conflict in the C++ file. The command function is called ``SetPowerLevel_cmdHandler``,
and the port function is called ``SetPowerLevel_handler``.

Handle the power level
----------------------
When that ``SetPowerLevel_handler`` function is called, the power level percent can be found in the ``percent`` parameter variable.
The Thruster component should respond the same way to setting the power over a port as it does to setting the power through a command from the GDS.

One solution would be to copy the body of the ``SetPowerLevel_cmdHandler`` function and paste it into the body of the ``SetPowerLevel_handler`` function.
This is a bad solution, though, because it creates **duplicate code**, which is a violation of the `D.R.Y. <https://en.wikipedia.org/wiki/Don%27t_repeat_yourself>`_ principle.

Instead we'll do some `refactoring <https://en.wikipedia.org/wiki/Code_refactoring>`_ and move the code inside the ``SetPowerLevel_cmdHandler`` function into its own function,
then that function can be called from both the ``SetPowerLevel_cmdHandler`` function and the ``SetPowerLevel_handler`` function.

Declare the ``setPowerLevel`` member function
---------------------------------------------
Edit the ``Thruster.hpp`` file and add a ``setPowerLevel`` function declaration.
You can put it at the end of the file, but be sure it's inside the class declaration code.

.. code-block:: c++

      void setPowerLevel(
          U8 percent
      );

I put mine just after the ``SetPowerLevel_cmdHandler`` declaration.

.. code-block:: c++

      void SetPowerLevel_cmdHandler(
          const FwOpcodeType opCode, /*!< The opcode*/
          const U32 cmdSeq, /*!< The command sequence number*/
          U8 percent 
      );

      void setPowerLevel(
          U8 percent
      );

  };

Define the ``setPowerLevel`` member function
--------------------------------------------
Edit the ``Thruster.cpp`` file and add a ``setPowerLevel`` function definition.
You can put it at the end of the file as well.

.. code-block:: c++

  void Thruster::
    setPowerLevel(
        U8 percent
    )
  {
  }

Build it
~~~~~~~~
Whenever I make a significant change to a source file, I like to compile it to be sure I'm doing it correctly, even if I'm not finished with it.
Enter the ``fprime-util build`` command in the ``Thruster`` directory to be sure you've written the function correctly so far.

Fill in the ``setPowerLevel`` function
--------------------------------------
This is where the refactoring will take place.
The statements inside the ``SetPowerLevel_cmdHandler`` will be moved into the ``setPowerLevel`` function.

Find the ``SetPowerLevel_cmdHandler`` function in the ``Thruster.cpp`` file.
Using your text editor, select all the statements starting at the beginning of the function (starting with ``if (_powerStatus == Fw::On::ON) {``), and ending on the line *before* the final line of the function
The final line is this, and you must not select this line:

.. code-block:: c++

    this->cmdResponse_out(opCode,cmdSeq,Fw::CmdResponse::OK);

Thus, you should have this block selected:

.. code-block:: c++

    if (_powerStatus == Fw::On::ON) {
        ...
        (12 lines ommitted)
        ...
    else {
      this->log_WARNING_LO_LevelSetPowerOffWarning();
    }

*Cut* that block of statements and paste them directly into the ``setPowerLevel`` function.
That function should now look like this:

.. code-block:: c++

  void Thruster::
    setPowerLevel(
        U8 percent
    )
  {
    if (_powerStatus == Fw::On::ON) {
      this->_powerLevelPercent = percent;
      this->log_ACTIVITY_HI_PowerLevelSet(percent);
      // check the warn limit
      Fw::ParamValid paramValid;
      Fw::Enabled isEnabled = this->paramGet_ShowPowerLevelHiWarn(paramValid);
      if (paramValid.isValid() && isEnabled == Fw::Enabled::ENABLED) {
        U8 hiWarnLimitPercent = this->paramGet_PowerLevelHiWarnPercent(paramValid);
        if (paramValid.isValid() && percent > hiWarnLimitPercent) {
          this->log_WARNING_HI_PowerLevelHiWarn(percent, hiWarnLimitPercent);
        }
      }
    }
    else {
      this->log_WARNING_LO_LevelSetPowerOffWarning();
    }
  }

Make sure that this statement:

.. code-block:: c++

    this->cmdResponse_out(opCode,cmdSeq,Fw::CmdResponse::OK);

Is still in the ``SetPowerLevel_cmdHandler`` function, and is not in the ``setPowerLevel`` function.

Call the ``setPowerLevel`` function
-----------------------------------
Add a call to the ``setPowerLevel`` function inside the ``SetPowerLevel_cmdHandler`` function, like this:

.. code-block:: c++

  void Thruster ::
    SetPowerLevel_cmdHandler(
        const FwOpcodeType opCode,
        const U32 cmdSeq,
        U8 percent
    )
  {
    this->setPowerLevel(percent);
    this->cmdResponse_out(opCode,cmdSeq,Fw::CmdResponse::OK);
  }

and do the same in the ``SetPowerLevel_handler`` function.

.. code-block:: c++

  void Thruster ::
    SetPowerLevel_handler(
        const NATIVE_INT_TYPE portNum,
        U8 percent
    )
  {
    setPowerLevel(percent);
  }

Now there are two ways to set the power level of the thruster:

#. From a command from the GDS.
#. Over a port from another component.

Build it
--------
.. code-block:: bash

    fprime-util build

You should see this:

.. code-block:: text

    [100%] Built target Components_Thruster

Conclusion
----------
The Thruster's power level can now be controlled by another component over a port,
but the ThrusterController doesn't yet have the ability to send a port command to the Thruster.
We'll add that feature next.
