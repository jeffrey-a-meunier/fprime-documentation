Modify the topology
===================

A topology instantiates all the components in a running system and links them together.
Many of the automatically-generated ports for commands, events, and telemetry are connected automatically.
In addition, the topology specifies how to construct the component instance.

In order to add a component to the topology, it must be added to the topology model.
An instance definition and an instance initializer must also be added.

Create a component instance
---------------------------
An instance initializer must be added to topology instances defined in the ``SpacecraftDeployment/Top/instances.fpp`` file.
Since the ``Thruster`` component is an *active* component it should be added to the active components section and should define a priority and queue depth options.

Add to ``SpacecraftDeployment/Top/instances.fpp``:

.. code-block:: text

    ...
    # ------------------------------------------------------------------
    # Active component instances
    # ------------------------------------------------------------------
    instance ...
        ...
        ...
        ...
        
    instance ...
    
    instance thruster: Components.Thruster base id 0x0F00 \
        queue size Default.QUEUE_SIZE \
        stack size Default.STACK_SIZE \
        priority 50

This means that we're creating a new instance called ``thruster`` that is an instance of the ``Components.Thruster`` type.
Remember that the ``Thruster`` component was defined in the ``Components`` namespace in the ``Thruster.fpp`` file:

.. code-block:: text

    module Components {
        @ Spacecraft thruster.
        active component Thruster {

Ensure that the base id (0x0F00) does not conflict with any other base ids in the topology.

Add the instance to the topology
--------------------------------
Edit the ``topology.fpp`` file found in the ``SpacecraftDeployment/Top`` directory
and add ``instance thruster`` somewhere in the list of instances (it does not need to be in alphabetical order).

Note that this instance name must match exactly the instance name that we used in the ``instances.fpp`` file

.. code-block:: text

    ...
    topology HelloWorldDeployment {
    # ---------------------------------------------------------------
    # Instances used in the topology
    # ---------------------------------------------------------------

    instance ...
    instance ...
    instance thruster

Tell CMake where to find the component
--------------------------------------
Locate the ``CMakeLists.txt`` file in the ``SpacecraftDeployment`` directory.
(There is a ``CMakeLists.txt`` files in several different directories -- be sure to select the correct one.)

Add the following line to that ``CMakeLists.txt`` file below the other two include statements in the file.

.. code-block:: cmake

    include("${CMAKE_CURRENT_LIST_DIR}/../project.cmake")

This includes the large project (e.g. Components) in this deployment's build.
