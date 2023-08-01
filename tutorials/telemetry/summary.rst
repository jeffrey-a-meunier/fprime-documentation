Summary
=======

``Thruster.fpp``
----------------
.. code-block:: text

    @ Target power level percent
    telemetry PowerStatus: Fw.On

    @ Actual power level percent
    telemetry PowerLevelPercent: U8

    @ Port to receive calls from the rate group
    async input port run: Svc.Sched

``Thruster.cpp``
----------------
.. code-block:: c++

    void Thruster ::
      run_handler(
          const NATIVE_INT_TYPE portNum,
          NATIVE_UINT_TYPE context
      )
    {
      this->tlmWrite_PowerStatus(this->_powerStatus);
      this->tlmWrite_PowerLevelPercent(this->_powerLevelPercent);
    }

``ThrusterController.fpp``
--------------------------
.. code-block:: text

    @ Target power level percent
    telemetry PowerLevelTargetPercent: U8

    @ Actual power level percent
    telemetry PowerLevelPercent: U8

``ThrusterController.cpp``
--------------------------
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

  void ThrusterController ::
    writeTelemetry()
  {
    this->tlmWrite_PowerLevelTargetPercent(this->m_targetPercent);
    this->tlmWrite_PowerLevelPercent(this->m_currentPercent);
  }

``topology.fpp``
----------------
.. code-block:: text

    connections SpacecraftDeployment {
      # Add here connections to user-defined components
      thrusterController.PowerLevelCommand -> thruster.SetPowerLevel
      rateGroup1.RateGroupMemberOut[3] -> thrusterController.run
      rateGroup1.RateGroupMemberOut[4] -> thruster.run
    }

``SpacecraftDeploymentPackets.xml``
-----------------------------------
.. code-block:: xml

    <packet name="Thruster" id="10" level="3">
        <channel name = "thruster.PowerStatus"/>
        <channel name = "thruster.PowerLevelPercent"/>
        <channel name = "thrusterController.PowerLevelTargetPercent"/>
        <channel name = "thrusterController.PowerLevelPercent"/>
    </packet>
