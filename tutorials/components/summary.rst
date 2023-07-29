Summary of changes
==================

1. ``cd ~/workspace/Spacecraft``
2. ``mkdir Components``
3. ``cd Components``
4. ``fprime-util new --component``, name ``Thruster``, description ``Spacecraft thruster``, namespace ``Components``, defaults for the rest.
5. ``cd Thruster``
6. ``fprime-util impl``
7. ``mv Thruster.hpp-template Thruster.hpp``
8. ``mv Thruster.cpp-template Thruster.cpp``
9. ``fprime-util build``
10. ``cd ~/workspace/Spacecraft``
11. ``fprime-util new --deployment``
12. Edit ``~/workspace/Spacecraft/SpacecraftDeployment/Top/instances.fpp``, add

.. code-block:: text

  instance thruster: Components.Thruster base id 0x0F00 \
    queue size Default.QUEUE_SIZE \
    stack size Default.STACK_SIZE \
    priority 50

13. Edit ``~/workspace/Spacecraft/SpacecraftDeployment/Top/tpology.fpp``, add

.. code-block:: text

    instance thruster

14. Edit ``~/workspace/Spacecraft/SpacecraftDeployment/CMakeLists.txt``, add

.. code-block:: text

    include("${CMAKE_CURRENT_LIST_DIR}/../project.cmake")

15. ``cd ~/workspace/Spacecraft/SpacecraftDeployment``
16. ``fprime-util build -j4``
17. ``fprime-gds --gui-addr 0.0.0.0``
