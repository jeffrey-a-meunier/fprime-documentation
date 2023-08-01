Summary
=======

``ThrusterController.fpp``
--------------------------
.. code-block:: text

    @ Port to receive calls from the rate group
    async input port run: Svc.Sched

``ThrusterController.hpp``
--------------------------
.. code-block:: c++

    U8 m_currentPercent;
    U8 m_targetPercent;

.. code-block:: c++

    //! Handler implementation for run
    //!
    void run_handler(
        const NATIVE_INT_TYPE portNum, /*!< The port number*/
        NATIVE_UINT_TYPE context /*!< 
    The call order
    */
    );

``ThrusterController.cpp``
--------------------------
.. code-block:: c++

  ThrusterController ::
  ThrusterController(
      const char *const compName
  ) : ThrusterControllerComponentBase(compName),
      m_currentPercent(0),
      m_targetPercent(0)
  {

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

``topology.fpp``
----------------
.. code-block:: text

    connections SpacecraftDeployment {
      # Add here connections to user-defined components
      thrusterController.PowerLevelCommand -> thruster.SetPowerLevel
      rateGroup1.RateGroupMemberOut[3] -> thrusterController.run
    }
