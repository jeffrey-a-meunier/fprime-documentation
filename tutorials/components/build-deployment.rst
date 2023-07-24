Build the deployment
====================

The build cache does not need to be generated this time because you did it already in the previous tutorial.

Make sure you're in the ``SpacecraftDeployment`` directory for the following commands.

Build
-----
The final executable file can now be built using this command:

.. code-block:: bash

    fprime-util build -j4

This build takes a few minutes.

This line is emitted somewhere in the output:

.. code-block:: bash

    [100%] Linking CXX executable bin/Linux/SpacecraftDeployment

That's the location of the actual executable file.

You should also see this last line of the output:

.. code-block:: bash

    [100%] Built target SpacecraftDeployment
