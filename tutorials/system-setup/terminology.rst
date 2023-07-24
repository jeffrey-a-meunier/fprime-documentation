Terminology
===========
F' uses specific terminology to refer to specific parts of the system.
This section dives into the basic F' terminology used in this tutorial and an explanation of how the terminology is used.

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
Commands represent actions that a component can execute.
Commands can be sent from the ground or via command sequences.
Command behavior is defined in a command handler defined in the component's implementation.

Event
-----
Events represent actions that a component has performed.
Events are akin to software log messages.
Events are received and displayed by the ground system.

Telemetry Channel
-----------------
Telemetry channels provide the active state of the component.
Each channel represents a single item of state that will be sent to the GDS (ground data system).

Deployment
----------
Deployments are a single build of F' software.
Each deployment results in one software executable that can be run along with other associated files.
Each deployment has a topology that defines the system.

In this tutorial you'll create the `SkeletonDeployment` deployment, run it, and test it through the F' GDS.

Topology
--------
Topologies are networks of connected components that define the software.
Multiple instances of a component may be added to the topology.
