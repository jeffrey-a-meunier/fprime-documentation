Troubleshooting: ``No declaration matches``
===========================================

Problem
-------

.. code-block:: text

    /home/jeff/workspace/Spacecraft/Components/Thruster/Thruster.cpp:59:8: error: no declaration matches ‘void Components::Thruster::IsPowerOn_cmdHandler(FwOpcodeType, U32, bool)’
    59 |   void Thruster ::
        |        ^~~~~~~~
    /home/jeff/workspace/Spacecraft/Components/Thruster/Thruster.cpp:59:8: note: no functions named ‘void Components::Thruster::IsPowerOn_cmdHandler(FwOpcodeType, U32, bool)’
    In file included from /home/jeff/workspace/Spacecraft/Components/Thruster/Thruster.cpp:8:
    /home/jeff/workspace/Spacecraft/Components/Thruster/Thruster.hpp:14:9: note: ‘class Components::Thruster’ defined here
    14 |   class Thruster :
        |         ^~~~~~~~
    gmake[3]: *** [Components/Thruster/CMakeFiles/Components_Thruster.dir/build.make:95: Components/Thruster/CMakeFiles/Components_Thruster.dir/Thruster.cpp.o] Error 1

Solution
--------
There is no declaration for that function in the ``.hpp`` file.

Did you copy the command handler from the ``.cpp-template`` file, but forget to copy the header declaration from the ``.hpp-template`` file?
