Edit commands
=============

The parameters have now been added to our project, so now the commands can use them.

Examine the autocoded functions
-------------------------------
In your project directory there is a directory called ``build-fprime-automatic-native``.
This directory contains all the auto-coded C++ files.
Change into that directory, and then into the ``Components/Thruster`` directory.
View the contents of the ``ThrusterComponentAc.hpp`` file and look for these functions:

.. code-block:: c++

    //! Get parameter ShowPowerLevelHiWarn
    //!
    /* Parameter to switch power level warning on or off */
    //! \return The parameter value
    //!
    Fw::Enabled paramGet_ShowPowerLevelHiWarn(
        Fw::ParamValid& valid /*!< Whether the parameter is valid*/
    );

    //! Get parameter PowerLevelHiWarnPercent
    //!
    /* Parameter to indicate the level at which to issue the power level warning */
    //! \return The parameter value
    //!
    U8 paramGet_PowerLevelHiWarnPercent(
        Fw::ParamValid& valid /*!< Whether the parameter is valid*/
    );

Observe the return type of each function:

* paramGet_ShowPowerLevelHiWarn returns a Fw::Enabled.
* paramGet_PowerLevelHiWarnPercent returns a U8.

This is just as we specified those parameters in the ``.fpp`` file.

Each of the functions has a reference parameter of type ``Fw::ParamValid``.

.. note::

  A *reference parameter* is passed by *reference*, which means that the function is able to store a value into that variable,
  and the caller will see the new value in that variable when the function returns.
  This is a secondary way for a function to return information.

This ``Fw::ParamValid`` type has a member function called ``isValid()`` that lets us check if the parameter value is valid or not.

.. note::

    FIXME: How does a parameter value become valid or in-valid?

Modify ``SetPowerLevel_cmdHandler``
-----------------------------------
Here you'll modify the ``SetPowerLevel_cmdHandler`` function to check the values of the two new parameters.
If the warning is enabled and the power level is above the hi-warn value, then a warning event will be sent to the GDS.

Find this line in the function:

.. code-block:: c++

  this->log_ACTIVITY_HI_PowerLevelSet(percent);

And add these lines just below that line:

.. code-block:: c++

      // check the warn limit
      Fw::ParamValid paramValid;
      Fw::Enabled isEnabled = this->paramGet_ShowPowerLevelHiWarn(paramValid);
      if (paramValid.isValid() && isEnabled == Fw::Enabled::ENABLED) {
        U8 hiWarnLimitPercent = this->paramGet_PowerLevelHiWarnPercent(paramValid);
        if (paramValid.isValid() && percent > hiWarnLimitPercent) {
          this->log_WARNING_HI_PowerLevelHiWarn(percent, hiWarnLimitPercent);
        }
      }

The body of the function should now look like this:

.. code-block:: c++

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
    this->cmdResponse_out(opCode,cmdSeq,Fw::CmdResponse::OK);
  }
