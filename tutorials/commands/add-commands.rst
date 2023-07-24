Add commands
============
In this section you will add two commands to the ``Thruster`` component, enabling it to be turned on and off.

Replace the TODO command
------------------------
Start a text editor and edit the ``Thruster.fpp`` file in the ``Components/Thruster`` directory.

Locate the existing TODO command:

.. code-block:: text

    # One async command/port is required for active components
    # This should be overridden by the developers with a useful command/port
    @ TODO
    async command TODO opcode 0

Replace it with these three commands:

.. code-block:: text

    @ Command to turn on the thruster
    async command PowerOn()

    @ Command to turn off the thruster
    async command PowerOff()

    @ Command to set the thruster power level
    async command SetPowerLevel(percent: U8)

The ``percent: U8`` parameter is an unsigned 8-bit integer that will be used as a percentage value from 0 to 100.

Generate the implemetation
--------------------------
In the ``Components/Thruster`` directory, enter this command:

.. code-block:: bash

    fprime-util impl

This will convert the ``.fpp`` file into ``.cpp`` and ``.hpp`` files.
However, the ``impl`` comand creates *template* ``.cpp`` and ``.hpp`` files that need to be renamed.

Execute these two commands to rename the template files:

.. code-block:: bash

    $ mv Thruster.cpp-template Thruster.cpp
    $ mv Thruster.hpp-template Thruster.hpp

Edit ``Thruster.hpp``
---------------------
Open the ``Thruster.hpp`` file in an editor.
Look near the bottom and you will see declarations for the three new commands we added.

.. code-block:: c++

    void PowerOn_cmdHandler(
        const FwOpcodeType opCode, /*!< The opcode*/
        const U32 cmdSeq /*!< The command sequence number*/
    );

    void PowerOff_cmdHandler(
        const FwOpcodeType opCode, /*!< The opcode*/
        const U32 cmdSeq /*!< The command sequence number*/
    );

    void SetPowerLevel_cmdHandler(
        const FwOpcodeType opCode, /*!< The opcode*/
        const U32 cmdSeq, /*!< The command sequence number*/
        U8 percent 
    );

Each defined command has a unique opcode, and each time a command is issued from the GDS it's given a monotonically increasing sequence number.

Just above the command declarations, add this private member variable:

.. code-block:: c++

    U8 _powerLevelPercent;

Save the contents of the file.

Edit ``Thruster.cpp``
---------------------
Open the ``Thruster.cpp`` file in an editor.

Locate the constructor and add an initialization statement for the ``_powerLevelPercent`` variable.
Add the line ``_powerLevelPercent(0)`` to the constructor, as shown below.

Don't forget to add a comma at the end of the line before it!

.. code-block:: c++

    Thruster ::
    Thruster(
        const char *const compName
    ) : ThrusterComponentBase(compName),
        _powerLevelPercent(0)
  {

  }

Locate the three command handler functions.

.. code-block:: c++

  void Thruster ::
    PowerOn_cmdHandler(
        const FwOpcodeType opCode,
        const U32 cmdSeq
    )
  {
    // TODO
    this->cmdResponse_out(opCode,cmdSeq,Fw::CmdResponse::OK);
  }

  void Thruster ::
    PowerOff_cmdHandler(
        const FwOpcodeType opCode,
        const U32 cmdSeq
    )
  {
    // TODO
    this->cmdResponse_out(opCode,cmdSeq,Fw::CmdResponse::OK);
  }

  void Thruster ::
    SetPowerLevel_cmdHandler(
        const FwOpcodeType opCode,
        const U32 cmdSeq,
        U8 percent
    )
  {
    // TODO
    this->cmdResponse_out(opCode,cmdSeq,Fw::CmdResponse::OK);
  }

Currently, each of these command handler functions does nothing other than acknowledge to the caller (which is the GDS) that it has completed successfully.

Replace the ``// TODO`` comment in the ``SetPowerLevel_cmdHandler`` function with an assignment statement that stores the value of the percent parameter variable into the corresponding member variable that you added in the header file.

.. code-block:: c++

  {
    _powerLevelPercent = percent;
    this->cmdResponse_out(opCode,cmdSeq,Fw::CmdResponse::OK);
  }

This stores the value of the parameter variable into the member variable, making that value avaialble to all other class member functions.

Build the component
-------------------
Now that we've modified the ``.hpp`` and ``.cpp`` files we need to build the component.
Enter this command in the ``Components/Thruster`` directory:

.. code-block:: bash

    fprime-util build

Proceed to the next section only if you see this:

.. code-block:: text

    [100%] Built target Components_Thruster

A note about variable names
---------------------------
Notice that I gave the member variable a long and descriptive name:

.. code-block:: c++

    _powerLevelPercent

(and I also like to start my member variables with underscores), whereas the function parameter is much shorter:

.. code-block:: c++

    percent

In the context of the class declaration, I wanted to indicate to readers the full purpose of the member variable: it's the thruster's *power level percent*.
There's no better way for the reader to understand fully the purpose of that variable.

In contrast, the ``percent`` parameter variable already has a descriptive context, which is the name of the function: ``SetPowerLevel_cmdHandler``.
In that function it would be redundant to use either of the words ``power`` or ``level`` in the variable name, or even ``thruster`` because we already know what component this is.
It is useful, though, to remind whomever calls the function what unit to use for the value:
In this case we're expecting a *percent*, which is a number in the range 0 to 100.
