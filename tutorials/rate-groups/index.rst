Tutorial 7: Rate groups
=======================

This tutorial uses a rate group handler to send telemetry to the GDS.

The next tutorial will introduce ports, and use a rate group to send port messages between components.

https://nasa.github.io/fprime/UsersGuide/best/rate-group.html

Rate group membership example:

https://github.com/nasa/fprime/blob/ddcb2ec138645da34cd4c67f250b67ee8bc67b26/Ref/Top/topology.fpp#L97-L124

.. note::

   Where is a rate group defined? I mean the rate group's rate? How do you know what a rate group's rate is?

   * It's in a component called RateGroup.

.. toctree::
   :maxdepth: 1
   :caption: Contents:
   :numbered:

   existing-project
   add-telemetry
   add-rate-group-port
   edit-commands
   edit-topology
   build-deployment
   run-gds
