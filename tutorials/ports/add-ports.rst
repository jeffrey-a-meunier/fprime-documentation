Add ports
=========
A port is used to send information from one component to another.
The component that sends information must have an output port defined.
The component that receives information must have an input port defined having the same type as the output port.

In the next section we'll modify the topology to connect the output port to the input port.

Create an input port type
-------------------------
In the Thruster component we'll define a port type and then create an input port.

Edit the ``Thruster.fpp`` file and add this port type definition just inside the module, but before the component definition.

.. code-block:: text

    @ Port for sending and receiving power level percent values
    port PowerLevelPort(percent: U8)

The file should now look like this.

.. code-block:: text

    module Components {

        @ Port for sending and receiving power level percent values
        port PowerLevelPort(percent: U8)

        @ Spacecraft thruster.
        active component Thruster {

We'll use this same port type in the ``ThrusterComponent``,
but since ``PowerLevelPort`` is declared inside the ``Components`` module,
it becomes available to all other components defined in that module automatically.

Create an input port
--------------------
In the ``Thruster.fpp`` file create a new port.

.. code-block:: text

    async input port setPowerLevel