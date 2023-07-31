Tutorial 6: Ports
=================

An F' deployment typically consists of several component instantiations running at the same time.
Components communicate with each other through **ports**.

In this tutorial you'll create a **ThrusterController** component that will allow the GDS to set the thruster to one of 5 pre-define levels:

+------+-------+
| Name | Value |
+======+=======+
| OFF  | 0%    |
+------+-------+
| LOW  | 25%   |
+------+-------+
| MED  | 50%   |
+------+-------+
| HI   | 75%   |
+------+-------+
| MAX  | 100%  |
+------+-------+

When the **ThrusterController** component receives a power level command of either **OFF**, **LOW**, **MED**, **HI**, or **MAX**,
it will send message over a port to the **Thruster** component with the correct percent power level setting.

.. toctree::
   :maxdepth: 1
   :caption: Contents:
   :numbered:

   existing-project
   create-thruster-controller
   add-thruster-input-port
   add-thruster-controller-output-port
   add-presets-command-event
   copy-template-definitions
   write-to-output-port
   create-thruster-controller-instance
   modify-topology
   build-deployment
   run-gds
   summary
