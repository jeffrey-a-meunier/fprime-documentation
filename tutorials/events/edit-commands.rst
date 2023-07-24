Edit the commands
=================

Here's the point where you call the event functions inside the command functions.

The component will invoke an event each time one of the new commands is called.

The component will invoke a warning event if the power level is set to a new value while the thruster is powered off.

Add member variable
-------------------
In the previous tutorial I forgot to add a member variable to keep track of whether the thruster's power is on or off.
We'll add that first, then after that we'll add calls to the event functions inside the command handlers.

Edit the ``Thruster.hpp`` file and add this member variable in the private section.
You can add it just before or after the ``_powerLevelPercent`` variable.

.. code-block:: c++

    Fw::On _powerStatus;

Initialize the ``_powerStatus`` variable
----------------------------------------
Edit the ``Components/Thruster.cpp`` file. Initialize the ``_powerStatus`` variable in the constructor and set it to ``OFF``.

.. code-block:: c++

  Thruster ::
    Thruster(
        const char *const compName
    ) : ThrusterComponentBase(compName),
        _powerLevelPercent(0),
        _powerStatus(Fw::On::OFF)
  {

  }

Modify command handlers
-----------------------
Now we'll use the ``_powerStatus`` member variable in the command handlers.

PowerOn
~~~~~~~
Add an assignment statement to the ``PowerOn`` command handler to set the ``_powerStatus`` variable to ``Fw::On::ON``.
Also in this function, call the ``log_ACTIVITY_HI_PowerOn`` event function.

.. code-block:: c++

  void Thruster ::
    PowerOn_cmdHandler(
        const FwOpcodeType opCode,
        const U32 cmdSeq
    )
  {
    _powerStatus = Fw::On::ON;
    this->log_ACTIVITY_HI_PowerOn();
    this->cmdResponse_out(opCode,cmdSeq,Fw::CmdResponse::OK);
  }

PowerOff
~~~~~~~~
Add an assignment statement to the ``PowerOff`` command handler to set the ``_powerStatus`` variable to ``Fw::On::OFF``.
Also in this function, call the ``log_ACTIVITY_HI_PowerOff`` event function.

.. code-block:: c++

  void Thruster ::
    PowerOff_cmdHandler(
        const FwOpcodeType opCode,
        const U32 cmdSeq
    )
  {
    _powerStatus = Fw::On::OFF;
    this->log_ACTIVITY_HI_PowerOff();
    this->cmdResponse_out(opCode,cmdSeq,Fw::CmdResponse::OK);
  }

PowerStatus
~~~~~~~~~~~
The ``PowerStatus`` command handler simply reports an event back to the GDS indicating the On/Off status of the thruster's power.

.. code-block:: c++

  void Thruster ::
    PowerStatus_cmdHandler(
        const FwOpcodeType opCode,
        const U32 cmdSeq
    )
  {
    this->log_ACTIVITY_HI_powerStatus(_powerStatus);
    this->cmdResponse_out(opCode,cmdSeq,Fw::CmdResponse::OK);
  }

SetPowerLevel
~~~~~~~~~~~~~
The ``SetPowerLevel`` command handler is a little more complicated.
We'll check the value of the ``_powerStatus`` member variable.
If it's ``ON``, then that means the thruster is powered on and we will allow the power level to be changed.
If it's ``OFF`` then we'll send a waring event to the GDS.

.. code-block:: c++

  void Thruster ::
    SetPowerLevel_cmdHandler(
        const FwOpcodeType opCode,
        const U32 cmdSeq,
        U8 percent
    )
  {
    if (_powerStatus == Fw::On::ON) {
      this->_powerLevelPercent = percent;
      this->log_ACTIVITY_HI_PowerLevelSet(percent);
    }
    else {
      this->log_WARNING_LO_LevelSetPowerOffWarning();
    }
    this->cmdResponse_out(opCode,cmdSeq,Fw::CmdResponse::OK);
  }

Additional features
-------------------
Currently there's no range check on the value in the ``percent`` parameter variable.
Since the variable's type is U8, it can range from 0 to 255.

* What do you think should happen if the value is > 100?
* Should a new event be added to report the range violation back to the GDS?
