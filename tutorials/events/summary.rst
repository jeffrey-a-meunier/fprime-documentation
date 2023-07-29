Summary
=======

1. Edit ``Components/Thruster/Thruster.fpp`` and add this command:

.. code-block:: text

    @ Command to fetch the status of the thruster power
    async command PowerStatus()

2. Still editing ``Thruster.fpp``, replace ``ExampleEvent`` with these events:

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

3. In ``Components/Thruster`` execute ``fprime-util impl``
4. Edit ``Thruster.hpp``, add private variable ``Fw::On _powerStatus;``
5. Edit ``Thruster.cpp``, add ``_powerStatus(Fw::On::OFF)`` to constructor initializations
6. Still editing ``Thruster.cpp`` in ``PowerOn_cmdHandler``, add this line below ``_powerStatus = Fw::On::ON;``:

.. code-block:: c++

    this->log_ACTIVITY_HI_PowerOn();

7. Add this line to ``PowerOff_cmdHandler``:

.. code-block:: c++

    this->log_ACTIVITY_HI_PowerOff();

8. In ``PowerStatus_cmdHandler``, add this as the first line of the body of the function:

.. code-block:: c++

    this->log_ACTIVITY_HI_powerStatus(_powerStatus);

9. Make this the body of the ``SetPowerLevel_cmdHandler`` function:

.. code-block:: c++

  if (_powerStatus == Fw::On::ON) {
    this->_powerLevelPercent = percent;
    this->log_ACTIVITY_HI_PowerLevelSet(percent);
  }
  else {
    this->log_WARNING_LO_LevelSetPowerOffWarning();
  }
  this->cmdResponse_out(opCode,cmdSeq,Fw::CmdResponse::OK);

10. In ``SpacecraftDeployment`` execute ``fprime-util build -j4``
11. ``fprime-gds --gui-addr 0.0.0.0``
