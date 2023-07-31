Summary
=======

1. Edit ``Thruster.fpp`` and replace the example parameter with:

.. code-block:: text

    @ Parameter to enable or disable the power level warning
    param ShowPowerLevelHiWarn: Fw.Enabled default Fw.Enabled.DISABLED

    @ Parameter to indicate the level at which to issue the power level warning
    param PowerLevelHiWarnPercent: U8 default 100

2. Add event to ``Thruster.fpp``

.. code-block:: text

  event PowerLevelHiWarn(limit: U8, level: U8) \
    severity warning high \
    format "Thruster level {}% is above the hi-warn limit {}%"

3. ``fprime-util impl``
4. ``fprime-util build``
5. In ``Thruster.cpp`` add this to ``SetPowerLevel_cmdHandler``

.. code-block:: text

    // check the warn limit
    Fw::ParamValid paramValid;
    Fw::Enabled isEnabled = this->paramGet_ShowPowerLevelHiWarn(paramValid);
    if (paramValid.isValid() && isEnabled == Fw::Enabled::ENABLED) {
      U8 hiWarnLimitPercent = this->paramGet_PowerLevelHiWarnPercent(paramValid);
      if (paramValid.isValid() && percent > hiWarnLimitPercent) {
        this->log_WARNING_HI_PowerLevelHiWarn(percent, hiWarnLimitPercent);
      }
    }

6. In ``SpacecraftDeployment``, execute ``fprime-util build -j4``
7. ``fprime-gds --gui-addr 0.0.0.0``
