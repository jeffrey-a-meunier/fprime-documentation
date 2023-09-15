Terminology
===========

Project
-------
An F' project is a collection of files and folders used to work with F'.
At its core, a project is just a folder that is used to hold F' code.
Projects should specifically exclude the core F' library to avoid the "clone and own" problem.
Rather, projects should link-to a version of F'.

In this tutorial you'll create a new `Skeleton` project used to contain the other things created by the tutorial.

Component
---------
An F' component encapsulates a unit of system behavior.
Components use ports to communicate with other components and define commands, events, telemetry channels, and parameters.

Port
----
A port is an interface used to communicate between components.
This tutorial only uses built-in ports.

Command
-------
A command is an action that a component can execute.
A command can be sent from the ground, or executed in a group through a command sequence.
Command behavior is defined in a command handler defined in the component's implementation.

Event
-----
An event is a message sent from the component to the GDS (ground data system).

Telemetry Channel
-----------------
Telemetry channels provide information about the active state of the component.
Each telemetry channel represents a single item of state that will be sent to the GDS.

Deployment
----------
A deployment is a single build of your F' application.
Each deployment results in one executable file that can be run.
Each deployment also has a topology that defines the connections between the components.

Topology
--------
A topology is the set of components and connections between the comonents.
The topology also defines the instances of each component.
Multiple instances of a component may be added to the topology.
