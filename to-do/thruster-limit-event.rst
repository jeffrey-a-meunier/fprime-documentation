Thruster limit event
====================

In Thruster.fpp

I use the parameters backwards.
I have ``limit`` and ``level``, I think it should be ``level`` and ``limit``.

.. code-block:: text

        event PowerLevelHiWarn(limit: U8, level: U8) \
            severity warning high \
            format "Thruster level {}% is above the warn limit {}%"