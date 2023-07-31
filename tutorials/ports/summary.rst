Summary
=======

``Thruster.fpp``
----------------
File ``Spacecraft/Components/Thruster/Thruster.fpp``

.. code-block:: text

    @ Port to set the power level
    async input port SetPowerLevel: PowerLevelPort

``Thruster.hpp``
----------------
File ``Spacecraft/Components/Thruster/Thruster.hpp``

.. code-block:: text

      void SetPowerLevel_cmdHandler(
          const FwOpcodeType opCode, /*!< The opcode*/
          const U32 cmdSeq, /*!< The command sequence number*/
          U8 percent 
      );

      void setPowerLevel(
          U8 percent
      );

``Thruster.cpp``
----------------
File ``Spacecraft/Components/Thruster/Thruster.cpp``

.. code-block:: text

      void Thruster ::
    SetPowerLevel_handler(
        const NATIVE_INT_TYPE portNum,
        U8 percent
    )
  {
    setPowerLevel(percent);
  }

.. code-block:: text

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

.. code-block:: text

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

``ThrusterController.fpp``
--------------------------
File ``Spacecraft/Components/ThrusterController/ThrusterController.fpp``

.. code-block:: text

    enum PowerLevelPreset {
        OFF,
        LO,
        MED,
        HI,
        MAX
    }

.. code-block:: text

    @ Command to set the thruster power level
    async command SetPowerLevel(level: PowerLevelPreset)

.. code-block:: text

    event PowerLevelSet(level: PowerLevelPreset) \
        severity activity high \
        format "ThrusterController power level set to {}%"

.. code-block:: text

    output port PowerLevelCommand: PowerLevelPort

``ThrusterController.cpp``
--------------------------
File ``Spacecraft/Components/ThrusterController/ThrusterController.cpp``

.. code-block:: text

  void ThrusterController ::
    SetPowerLevel_cmdHandler(
        const FwOpcodeType opCode,
        const U32 cmdSeq,
        Components::ThrusterController_PowerLevelPreset level
    )
  {
    static const U8 presetPercents[] = {0, 25, 50, 75, 100};
    const U8 presetPercent = presetPercents[level.e];
    PowerLevelCommand_out(0, presetPercent);
    this->cmdResponse_out(opCode, cmdSeq,Fw::CmdResponse::OK);
  }

``instances.fpp``
------------------------------------------
File ``SpacecraftDeployment/Top/instances.fpp``

.. code-block:: text

  instance thruster: Components.Thruster base id 0x0F00 \
      queue size Default.QUEUE_SIZE \
      stack size Default.STACK_SIZE \
      priority 50

  instance thrusterController: Components.ThrusterController base id 0x1000 \
      queue size Default.QUEUE_SIZE \
      stack size Default.STACK_SIZE \
      priority 50

``topology.fpp``
----------------
File ``SpacecraftDeployment/Top/topology.fpp``

.. code-block:: text

    instance thruster
    instance thrusterController

.. code-block:: text

    connections SpacecraftDeployment {
      # Add here connections to user-defined components
      thrusterController.PowerLevelCommand -> thruster.SetPowerLevel
    }

