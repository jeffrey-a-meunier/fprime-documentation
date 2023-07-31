Edit the ``run-handler`` function
=================================

Before finishing the ``run-handler`` function we'll need to add two new member variables to the ``ThrusterController.hpp`` class.
These two member variables will hold the *current* thruster power level percent,
and the *target* thruster power level percent.

The ``run-handler`` function will gradually incremement the current thruster power level until it equals the target power level.

Add member variables
--------------------
Edit ``ThrusterController.hpp`` and add these two private member variables:

.. code-block:: c++

    U8 m_currentPercent;
    U8 m_targetPercent;

Initialize member variables
---------------------------
Edit ``ThrusterController.cpp``.
Add these two member variable initializations to the ThrusterController constructor.

.. code-block:: c++

  ThrusterController ::
    ThrusterController(
        const char *const compName
    ) : ThrusterControllerComponentBase(compName),
        m_currentPercent(0),
        m_targetPercent(0)
  {

Edit the ``run-handler``
------------------------
Now that we have the two member variables, we can use them in the ``run_handler`` function.
Add these lines to the ``run_handler`` function so that the function looks like this:

.. code-block:: c++

   void ThrusterController ::
    run_handler(
        const NATIVE_INT_TYPE portNum,
        NATIVE_UINT_TYPE context
    )
  {
    I16 delta = m_targetPercent - m_currentPercent;
    if (delta > 10) {
      delta = 10;
    }
    else if (delta < -10) {
      delta = -10;
    }
    m_currentPercent += delta;
    if (delta != 0) {
      PowerLevelCommand_out(0, m_currentPercent);
    }
  }

Notice what's happening here:

* A ``delta`` is being calculated, either positive or negative, and clamped to +/- 10%.
* The ``m_currentPercent`` variable is changed by this ``delta`` amount.
* The ``m_currentPercent`` variable is sent out the ``PowerLevelCommand`` port to the Thruster.

We will now let this function be the one that sends the power level to the Thruster.

The ``SetPowerLevel_cmdHandler`` function will now be changed so that instead of sending the power level percent value to the Thruster,
it sets the *target* thruster power level percent.

Change that function so that it looks like this:

.. code-block:: c++

  void ThrusterController ::
    SetPowerLevel_cmdHandler(
        const FwOpcodeType opCode,
        const U32 cmdSeq,
        Components::ThrusterController_PowerLevelPreset level
    )
  {
    static const U8 presetPercents[] = {0, 25, 50, 75, 100};
    const U8 presetPercent = presetPercents[level.e];
    m_targetPercent = presetPercent;
    this->cmdResponse_out(opCode, cmdSeq,Fw::CmdResponse::OK);
  }

Understand what happens when you send the ThrusterController a ``SetPowerLevel`` command:

* The ``SetPowerLevel_cmdHandler`` sets the ``m_targetPercent`` variable to that value.
* The ``run_handler`` function is called automatically once per second, and it handles the new value in the ``m_targetPercent`` variable.
