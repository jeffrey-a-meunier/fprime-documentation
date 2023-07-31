Copy template definitions
=========================

After running ``fprime-util impl`` in the previous section, new declarations are placed in the ``ThrusterController.hpp-template``,
and new defititions are placed in the ``ThrusterController.cpp-template`` file.

``SetPowerLevel`` command handler
---------------------------------
This command header declaration is in the ``ThrusterController.hpp-template`` file:

.. code-block:: c++

    //! Implementation for SetPowerLevel command handler
    //! Command to set the thruster power level
    void SetPowerLevel_cmdHandler(
        const FwOpcodeType opCode, /*!< The opcode*/
        const U32 cmdSeq, /*!< The command sequence number*/
        Components::ThrusterController_PowerLevelPreset level 
    );

Copy that and paste it into the ``ThrusterController.hpp`` file, replacing the ``TODO`` command handler.

Likewise, this command definition is in the ``ThrusterController.cpp-template`` file:

.. code-block:: c++

  void ThrusterController ::
    SetPowerLevel_cmdHandler(
        const FwOpcodeType opCode,
        const U32 cmdSeq,
        Components::ThrusterController_PowerLevelPreset level
    )
  {
    // TODO
    this->cmdResponse_out(opCode,cmdSeq,Fw::CmdResponse::OK);
  }

Copy that and paste it into the ``ThrusterController.cpp`` file, replacing the ``TODO`` command handler.

What about the new event?
-------------------------
The ``async command SetPowerLevel(level: PowerLevelPreset)`` event does have a C++ function associated with it, but it's not a function that you need to modify
so it's not placed in the ``-template`` files.

The new event function is placed in the auto-code C++ file that you can find here:

.. code-block:: text

  ~/workspace/Spacecraft/build-fprime-automatic-native/Components/ThrusterController/ThrusterControllerComponentAc.cpp

This is the beginning of the event function definition:

.. code-block:: c++

  void ThrusterControllerComponentBase ::
    log_ACTIVITY_HI_PowerLevelSet(
        Components::ThrusterController_PowerLevelPreset level
    )
  {
    ...
  }

Ensure that the component builds
--------------------------------
After modifying any of the source files (``.hpp`` or ``.cpp``) you should ``build`` in order to be sure there are no errors in them.

.. code-block:: bash

    fprime-util build

You should see this:

.. code-block:: text

    [100%] Built target Components_ThrusterController
