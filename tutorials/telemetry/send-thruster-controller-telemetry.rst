Send telemetry
==============

Sending telemetry in the ThrusterController component will be a little more complicated than it was in the Thruster component.

Edit the ``ThrusterController.cpp`` file and locate the ``run_handler`` function.

This is the existing ``run_handler`` function:

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

Currently that function already does one specific thing: it handles the power level and sends it to the Thruster component.
We need to add a second specific thing to it, namely sending telemetry to the GDS.

A well-designed function, however, should do only one specific thing.
If you find that you want to put a second thing into a function that's not directly related to the first thing, then you need a second function.

What we'll do here is move the code in the ``run_handler`` function into a new function called ``sendPowerLevel``,
and then create another new function called ``writeTelemetry``.
The ``run_handler`` function will then call both those functions.

Function declarations
---------------------
Add these two function declarations in the private section of the ``ThrusterController.hpp`` file.

.. code-block:: c++

    void sendPowerLevel();
    void writeTelemetry();

Define ``sendPowerLevel`` function
----------------------------------
Edit the ``ThrusterController.cpp`` file and move the statements from the body of the ``run_handler`` function and put them in a new ``sendPowerLevel`` function.
Then add a call to the ``sendPowerLevel()`` function in the ``run_handler`` function.

This is what it looks like:

.. code-block:: c++

  void ThrusterController ::
    run_handler(
        const NATIVE_INT_TYPE portNum,
        NATIVE_UINT_TYPE context
    )
  {
    this->sendPowerLevel();
  }

  void ThrusterController ::
    sendPowerLevel()
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

Define ``writeTelemetry`` function
----------------------------------
Create another new function called ``writeTelemetry`` and send the telemetry from it.

.. code-block:: c++

  void ThrusterController ::
    writeTelemetry()
  {
    this->tlmWrite_PowerLevelTargetPercent(this->m_targetPercent);
    this->tlmWrite_PowerLevelPercent(this->m_currentPercent);
  }

Then add a call to this function in the ``run_handler`` function.

.. code-block:: c++

  void ThrusterController ::
    run_handler(
        const NATIVE_INT_TYPE portNum,
        NATIVE_UINT_TYPE context
    )
  {
    this->sendPowerLevel();
    this->writeTelemetry();
  }

Build it
--------
Execute this command in the ``ThrusterController`` directory to be sure you typed everything correctly.

.. code-block:: bash

    fprime-util build
