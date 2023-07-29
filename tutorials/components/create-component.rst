Create a new component
======================
Make sure you're in the ``Spacecraft`` project directory and you've activated the Python virtual environment.

.. code-block:: bash

    cd ~/workspace/Spacecraft
    source venv/bin/activate

Make a component directory
--------------------------
For any F' project you must create a directory in which you place all your components,
then change into that directory.

.. code-block:: bash

    mkdir Components
    cd Components

Create a new component
----------------------
Enter this command:

.. code-block:: bash

    fprime-util new --component

* Enter ``Thruster`` as the component name.
* Enter ``Spacecraft thruster.`` as the description.
* Enter ``Components`` as the component namespace.
* Enter ``1`` for the component configuration options.
* Enter ``yes`` to the two yes/no questions.

.. code-block:: text

    [INFO] Cookiecutter source: using builtin
    component_name [MyComponent]: Thruster
    component_short_description [Example Component for F Prime FSW framework.]: Spacecraft thruster.
    component_namespace [Thruster]: Components
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
    [INFO] Found CMake file at 'ComponentExample/project.cmake'
    Add component Components/Thruster to ComponentExample/project.cmake at end of file (yes/no)? yes
    Generate implementation files (yes/no)? yes

A ``gmake`` warning is shown, but you're instructed to ignore the warning:

.. code-block:: text

    Refreshing cache and generating implementation files (ignore 'Stop' CMake warning)...
    gmake: *** No rule to make target 'Components_Thruster_impl'.  Stop.

There is a noticeable pause as the ``fprime-util`` utility does what it needs to do.
Finally you should see this:

.. code-block:: text

    [INFO] Created new component and generated initial implementations.

List the files
--------------
Change into the ``Thruster`` directory and then list the files.

.. code-block:: bash

    cd Thruster
    ls

You should see these files:

.. code-block:: text

    CMakeLists.txt
    Thruster.cpp
    Thruster.fpp
    Thruster.hpp
    docs

The ``.fpp`` file describes the behavior of the component using the declarative F-Prime-Prime language.
The ``.cpp`` and ``.hpp`` file are the preliminary component implementation files generated from the ``.fpp`` file.

Use a text editor to view the contents of the ``.fpp`` file.
You will see that there is one ``async command`` defined already called ``TODO``.

.. code-block:: text

    # One async command/port is required for active components
    # This should be overridden by the developers with a useful command/port
    @ TODO
    async command TODO opcode 0

In the next tutorial you will modify the ``.fpp`` file to add new commands,
but in this tutorial you'll use this command when you run the GDS (ground data system).

Generate the component implementation
-------------------------------------
Now you can create an implementation of the ``Thruster.fpp`` file by entering this command.
Be sure you're in the ``Thruster`` directory:

.. code-block:: text

    fprime-util impl

You should see these lines in the output:

.. code-block:: text

    [100%] Generating ../../../Components/Thruster/ThrusterComponentImpl.hpp-template, ../../../Components/Thruster/ThrusterComponentImpl.cpp-template
    ...
    [100%] Built target Components_Thruster_impl

That command creates ``Thruster.hpp-template`` and ``Thruster.cpp-template``, which contain empty functions based on the contents of the ``.fpp`` file.
While normally one would merge new templates with the existing code, we will instead overwrite the existing implementations as we have not edited those files yet.

Enter these two commands:

.. code-block:: bash

    mv Thruster.hpp-template Thruster.hpp
    mv Thruster.cpp-template Thruster.cpp

Build the component
-------------------
Enter this command to build ``Thruster``:

.. code-block:: text

    fprime-util build

You should see this at the end:

.. code-block:: text

    [100%] Linking CXX static library ../../lib/Linux/libComponents_Thruster.a
    [100%] Built target Components_Thruster

The ``Thruster`` component has now been compiled into a library file.
