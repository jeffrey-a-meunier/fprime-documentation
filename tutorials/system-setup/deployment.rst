Deployment
==========
This section will walk new users through creating a new F' deployment.
This deployment will build a topology containing the standard F' stack of components.
The tutorial will close by testing the deployment through the ``fprime-gds`` Ground Data System utility.

.. note::

    Be sure you've activated the Python virtual environment.
    Activate it if you do not see ``(venv)`` in your prompt.

    .. code-block:: bash

       ~/workspace/Spacecraft$ source venv/bin/activate
       (venv) ~/workspace/Spacecraft$
       ^^^^^^

Create a new deployment
-----------------------
A deployment is a single compiled instance of an F' project.
All the components created for an F' project run within a deployment.
The deployment created here will contain the standard command and data handling stack.
This stack enables ground control and data collection of the deployment.

To create a deployment, run the following commands in the ``Spacecraft`` directory:

.. code-block:: bash

    fprime-util new --deployment

This command will ask for some input. Respond with the following answers:

.. code-block:: text

    deployment_name [MyDeployment]: SpacecraftDeployment

For any other questions, select the default response.

At the end you should see a line like this:

.. code-block:: text

    [INFO] New deployment successfully created: /home/jeff/workspace/Spacecraft/SpacecraftDeployment

At this point, the ``SpacecraftDeployment`` has been created.

Generate and build
------------------
Change into the ``SpacecraftDeployment`` directory and enter this command:

.. code-block:: bash

    fprime-util generate

You should see these lines in the output:

.. code-block:: text

    -- Configuring done
    -- Generating done
    -- Build files have been written to: /home/jeff/workspace/Spacecraft/SpacecraftDeployment/build-fprime-automatic-native

Then enter this command:

.. code-block:: bash

    fprime-util build -j4

After a few minutes you should see this line in the output:

.. code-block:: text

    [100%] Built target SpacecraftDeployment
