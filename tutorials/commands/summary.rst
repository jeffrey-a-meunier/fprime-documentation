Summary
=======

1. Edit ``Components/Thruster/Thruster.fpp``
2. Replace the ``TODO`` command with:

.. code-block:: text

    @ Command to turn on the thruster
    async command PowerOn()

    @ Command to turn off the thruster
    async command PowerOff()

    @ Command to set the thruster power level
    async command SetPowerLevel(percent: U8)

3. In ``Components/Thruster`` execute ``fprime-util impl``
4. ``mv Thruster.cpp-template Thruster.cpp``
5. ``mv Thruster.hpp-template Thruster.hpp``
6. Edit ``Thruster.hpp``, add private variable ``U8 _powerLevelPercent;``
7. Edit ``Thruster.cpp``, add ``_powerLevelPercent(0)`` to constructor initializations
8. Still editing ``Thruster.cpp``, in ``SetPowerLevel_cmdHandler`` replace ``TODO`` with ``_powerLevelPercent = percent;``
9. ``fprime-util build``
10. ``cd ~/workspace/SpacecraftDeployment``
11. ``fprime-util build -j4``
12. ``fprime-gds --gui-addr 0.0.0.0``
