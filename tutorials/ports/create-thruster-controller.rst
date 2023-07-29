Create a ThrusterController component
=====================================

Change into the ``Spacecraft/Components`` directory and enter this command:

.. code-block:: bash

    fprime-util new --component

This is the transcript. Use the answers I've supplied here:

.. code-block:: text

    [INFO] Cookiecutter source: using builtin
    component_name [MyComponent]: ThrusterController
    component_short_description [Example Component for F Prime FSW framework.]: High-level controller for the Thruster.
    component_namespace [ThrusterController]: Components
    Select component_kind:
    1 - active
    2 - passive
    3 - queued
    Choose from 1, 2, 3 [1]: 
    Select enable_commands:
    1 - yes
    2 - no
    Choose from 1, 2 [1]: 
    Select enable_telemetry:
    1 - yes
    2 - no
    Choose from 1, 2 [1]: 
    Select enable_events:
    1 - yes
    2 - no
    Choose from 1, 2 [1]: 
    Select enable_parameters:
    1 - yes
    2 - no
    Choose from 1, 2 [1]: 
    [INFO] Found CMake file at 'Spacecraft/project.cmake'
    Already added to CMakeLists.txt
    Generate implementation files (yes/no)? yes
    Refreshing cache and generating implementation files (ignore 'Stop' CMake warning)...
    gmake: *** No rule to make target 'Components_ThrusterController_impl'.  Stop.
    [WARNING] clang-format not found in PATH. Skipping formatting.
    [INFO] Created new component and generated initial implementations.

