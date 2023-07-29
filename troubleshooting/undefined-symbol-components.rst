Troubleshooting: ``error: Undefined symbol Components``
=======================================================

Problem
-------
You see this error when you build the deployment:

.. code-block:: text

  [ 96%] Generating Ports_RateGroupsEnumAi.xml, Ports_StaticMemoryEnumAi.xml, SpacecraftDeploymentTopologyAppAi.xml
  fpp-to-xml
  /home/jeff/workspace/Spacecraft/SpacecraftDeployment/Top/instances.fpp: 85.22
    instance thruster: Components.Thruster base id 0x0F00 \
                       ^
  error: undefined symbol Components

Solution
--------
You forgot to edit the ``CMakeLists.txt`` file in the deployment directory.

Locate the ``F' Core Setup`` section and add the last line shown here:

.. code-block:: cmake

    ###
    # F' Core Setup
    # This includes all of the F prime core components, and imports the make-system.
    ###
    include("${FPRIME_FRAMEWORK_PATH}/cmake/FPrime.cmake")
    # NOTE: register custom targets between these two lines
    include("${FPRIME_FRAMEWORK_PATH}/cmake/FPrime-Code.cmake")

    include("${CMAKE_CURRENT_LIST_DIR}/../project.cmake")
