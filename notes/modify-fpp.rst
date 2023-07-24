Notes: When you modify the ``.fpp`` file
========================================
If you've already built the deployment but you modify the ``.fpp`` file, then these steps must be taken.

#. Reimplement the component:
   In the component directory run ``fprime-util impl``.
#. Edit the ``.cpp`` file as necessary (e.g., copy the new command from the template ``.hpp`` and ``.cpp`` files).
#. Rebuild the component:
   In the component directory run ``fprime-util build``.
#. Rebuild the deployment:
   In the deployment directory run ``fprime-util build``.
