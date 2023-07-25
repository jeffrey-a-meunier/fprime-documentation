Edit commands
=============

The parameters have now been added to our project, so the commands can used them.

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
    Fw::On paramGet_ShowPowerLevelHiWarn(
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
* paramGet_ShowPowerLevelHiWarn returns a Fw::On.
* paramGet_PowerLevelHiWarnPercent returns a U8.

This is just as we specified those parameters in the ``.fpp`` file.

Each of the functions has a reference parameter of type ``Fw::ParamValid``.
This type has a member function called ``isValid()`` that lets us check if the parameter value is valid or not.

.. note::

    FIXME: How does a parameter value become valid or in-valid?

Modify ``SetPowerLevel_cmdHandler``
-----------------------------------
Here you'll modify the ``SetPowerLevel_cmdHandler`` function to check the values of the two new parameters.
If the warning is enabled and the power leve is above the hi-warn value, then a warning event will be issued.

Replace the **consequent** section of the ``if (_powerStatus == Fw::On::ON)`` statement with what's shown here:
The new statements are shown below the ``// check the warn limit`` comment.

.. code-block:: c++

      this->_powerLevelPercent = percent;
      this->log_ACTIVITY_HI_PowerLevelSet(percent);
      // check the warn limit
      Fw::ParamValid paramValid;
      if (this->paramGet_ShowPowerLevelHiWarn(paramValid) && paramValid.isValid()) {
        U8 hiWarnLimitPercent = this->paramGet_PowerLevelHiWarnPercent(paramValid);
        if (paramValid.isValid() && percent > hiWarnLimitPercent) {
          this->log_WARNING_HI_PowerLevelHiWarn(hiWarnLimitPercent, percent);
        }
      }
