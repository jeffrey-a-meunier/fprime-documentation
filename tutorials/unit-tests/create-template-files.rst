Create template files
=====================

Edit the ``Components/Thruster/CMakeLists.txt`` file and add these lines at the bottom:

.. code-block:: CMake

    set(UT_SOURCE_FILES
      "${CMAKE_CURRENT_LIST_DIR}/Thruster.fpp"
    )
    register_fprime_ut()

Make sure you're in the Thruster component directory:

.. code-block:: bash

    $ cd Components/Thruster

Generate a build cache for testing:

.. code-block:: bash

    $ fprime-util generate --ut

That takes a few minutes to complete. You should see this at the end:

.. code-block:: text

    -- Build files have been written to: /home/jeff/workspace/Spacecraft/build-fprime-automatic-native-ut

Make test folders:

.. code-block:: bash

    mkdir -p test/ut

Generate template testing files by entering this command:

.. code-block:: bash

    fprime-util impl --ut

That takes a while to complete, as well. You should see this at the end:

.. code-block:: text

    [100%] Built target Components_Thruster_testimpl

That created four new files in the current directory:

* ``TestMain.cpp``
* ``Tester.cpp``
* ``Tester.hpp``
* ``TesterHelpers.cpp``

Move those files into the test folder:

.. code-block:: bash

    mv Test* test/ut

Build it:

.. code-block:: bash

    fprime-util build --ut

Run it:

.. code-block:: bash

    ../../build-fprime-automatic-native-ut/bin/Linux/Components_Thruster_ut_exe 

Currently there are no actual tests in the test program:

.. code-block:: text

    [==========] Running 0 tests from 0 test suites.
    [==========] 0 tests from 0 test suites ran. (0 ms total)
    [  PASSED  ] 0 tests.
