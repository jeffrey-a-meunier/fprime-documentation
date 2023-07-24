Create a new F' project
=======================
This section will have you create a new F' project.

Open a terminal window
----------------------
| If you're working natively (no VM):
| Open a Terminal window or Command window.

| If you're working inside Linux (using the Linux graphical desktop):
| Open a Terminal window.

| If you're using WSL (Windows Subsystem for Linux):
| Open a Command window and be sure to start **wsl**.

| If you're connecting remotely to the VM through SSH:
| Open a terminal window and SSH into the VM.

This is where you'll do all the work in these tutorials.

Install Dependencies
--------------------

Enter these commands:

.. code:: bash

   sudo apt update
   sudo apt upgrade
   sudo apt install python3-pip
   sudo apt install python3-venv
   sudo apt install git
   git config --global init.defaultBranch main
   sudo apt install cmake

You will also need to have a programmer's text editor installed.
I recommend Visual Studio Code (https://code.visualstudio.com), as it has useful extensions for F' and for C++.

If you do use VS Code, install the F' extension called **FPPTools by UNLV CS Senior Design Team**.

Install F'
----------
An F' project ties to a specific version of tools to work with F'.
In order to create this project and install the correct version of tools, an initial bootstrap version of F' tools must be installed.
This is accomplished with the following command:

.. code:: bash

   pip install fprime-tools

You will see several path warnings in yellow, like this:

.. code-block::

   WARNING: The script slugify is installed in '/home/jeff/.local/bin' which is not on PATH.
   Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.

Modify your shell script path to fix the problem that the warning is describing:

.. code-block:: bash

   echo "export PATH=$HOME/.local/bin:$PATH" >> ~/.bashrc 
   source ~/.bashrc 
   which fprime-util

That last command should show you where the ``fprime-util`` executable file is located:

.. code-block:: bash

   /home/jeff/.local/bin/fprime-util

Create a new project
--------------------
Now that the tools are installed a new F' project should be created.
An F' project internalizes the version of F' that the project will build upon and provides the user the basic setup for creating, building, and testing components.

Create a new workspace directory in which you'll create your F' project.
You can call this directory anything you like: ``source``, ``projects``, ``workspace``, ``fprime``, whatever you think makes sense.

.. code-block:: bash

   mkdir ~/workspace
   cd ~/workspace

Now run this command to create a new F' project.

.. code-block:: bash

   fprime-util new --project

This command will ask for some input. Respond with the following answers:

.. code-block:: text

   project_name [MyProject]: Spacecraft
   fprime_branch_or_tag [devel]: devel
   Select install_venv:
   1 - yes
   2 - no
   Choose from 1, 2 [1]: 1

If you are presented with additional options not shown above, just use the default values.
The command will take a moment to run as it needs to clone the core F' repository.

That command creates a new F' project structure in a folder called ``Spacecraft``,
uses the ``devel`` branch of F' as the basis for the project,
and sets up the matching tools in a new Python virtual environment.

.. note::

   It's possible that you'll see a large block of error text in red that starts like this:

   .. code-block:: text

      ERROR: Command errored out with exit status 1:
      command: /home/jeff/workspace/Spacecraft/venv/bin/python3 -u -c 'import sys, setuptools, tokenize; sys.argv[0] = '"'"'/tmp/pip-install-qy1phsza/fprime-fpp/setup.py'"'"'; __file__='"'"'/tmp/pip-install-qy1phsza/fprime-fpp/setup.py'"'"';f=getattr(tokenize, '"'"'open'"'"', open)(__file__);code=f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compile(code, __file__, '"'"'exec'"'"'))' bdist_wheel -d /tmp/pip-wheel-3gbox7_s

   It seems that this is not a fatal error. Proceed.

Ultimately you should see this displayed in the terminal window:

.. code-block:: text

   ################################################################

   Congratulations! You have successfully created a new F' project.

   A git repository has been initialized and F' has been added as a
   submodule, you can now create your first commit.

   Get started with your F' project:

   -- Activate the virtual environment --
   Linux/MacOS: source venv/bin/activate

   -- Generate a new component --
   fprime-util new --component

   -- Generate a new deployment --
   fprime-util new --deployment

   ################################################################

You are encouraged navigate to the project's directory and look around:

.. code-block:: bash

   cd Spacecraft
   ls

This will show the following files:

* ``fprime/``: F' repository. Contains core F' components, the API for the build system, among others.
* ``settings.ini``: allows users to set various settings to control the build.
* ``CMakeList.txt`` and ``project.cmake``: CMake files defining the build system.
* ``Components/``: Directory to place user components in. Some versions of F' do not create this directory.

Be sure you are still in the ``Spacecraft`` directory.
Execute this command in order to enter a Python virtual environment.
This environment gives you access to the tools needed for this project.

.. code-block:: bash

   source venv/bin/activate

Your terminal prompt should change to indicate that you are now working in a Python virtual environment.

.. code-block:: bash

   (venv) ~/workspace/Spacecraft$
   ^^^^^^

You must remember to activate the virtual environment any time you are working with this project.

Fix a package warning
---------------------
Uninstalling and re-installing the cheetah3 package in your virtual environment fixes a warning message that you would otherwise see.

.. code-block:: bash

   pip uninstall cheetah3
   pip install cheetah3

Building the project
--------------------
The next step is to set up and build the newly created project.
This will serve as a build environment for any newly created components,
and will build the F' framework supplied components.

Enter this command in the Spacecraft directory:

.. code-block:: bash

   fprime-util generate

The ``fprime-util generate`` command sets up the build environment for a project deployment.
It only needs to be done once when you start a new project.

You should see these lines at the end of the output:

.. code-block:: text

   -- Generating done
   -- Build files have been written to: /home/jeff/projects/Spacecraft/build-fprime-automatic-native

Then enter this command to build (compile) the project:

.. code-block:: bash

   fprime-util build -j4

The ``-j4`` argument to the build command is optional.
It instructs the make command to use up to 4 processor cores during compiling.

That command takes a little longer to complete.
You should see this at the end:

.. code-block:: text

   [100%] Linking CXX static library ../../lib/Linux/libUtils.a
   [100%] Built target Utils

Conclusion
----------
A new project has been created with the name ``Spacecraft``.
It includes the initial build system setup, and F' version.
