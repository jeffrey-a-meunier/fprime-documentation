What should we test?
====================
We'll start by testing the command handlers in the Thruster component.

Recall the PowerOn command handler:

.. code-block:: c++

  void Thruster ::
    PowerOn_cmdHandler(
        const FwOpcodeType opCode,
        const U32 cmdSeq
    )
  {
    _powerStatus = Fw::On::ON;
    this->log_ACTIVITY_HI_PowerOn();
    this->cmdResponse_out(opCode, cmdSeq, Fw::CmdResponse::OK);
  }

We'll write a test that ensures that three things happen whenever this command is called:

1. The ``_powerStatus`` member variable is set to ``Fw::On::ON``.
2. The ``PowerOn`` event is emitted.
3. The command response is set to ``Fw::CmdResponse::OK``.

After completing that test and ensuring that it works correctly, we'll write tests for the other commands as well:

* ``PowerOff_cmdHandler`` should be tested like ``PowerOn_cmdHandler``.
* ``SetPowerLevel_cmdHandler`` should be tested to ensure that it calls ``setPowerLevel``.
* ``setPowerLevel`` function is the most complex function in the component, so its behavior should be tested.
