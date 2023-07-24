Troubleshooting: Commands not available
=======================================

Problem
-------
You've added commands to your ``.fpp`` file but when you run the GDS, they don't appear in the **Mnemonic** list.

Solution
--------
It could be that the component instance is not running.

If you recently regenerated your deployment directory, then you need to and add the component to the toplogy.

* YourProjectDeployment/

  * Top/

    * instances.fpp
    * topology.fpp

  * CMakeLists.txt

Then you will need to do ``fprime-util generate`` and then ``fprime-util build`` in the deployment directory.

See tutorials/components/modify-topology.html
