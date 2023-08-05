Create template files and run tests
===================================

Edit the ``Components/Thruster/CMakeLists.txt`` file and add these lines at the bottom,
below ``register_fprime_module()``:

.. code-block:: CMake

    set(UT_SOURCE_FILES
      "${CMAKE_CURRENT_LIST_DIR}/Thruster.fpp"
    )
    set(UT_AUTO_HELPERS ON)
    register_fprime_ut()

Make sure you're in the Thruster component directory:

.. code-block:: bash

    cd Components/Thruster

Generate a build cache for testing:

.. code-block:: bash

    fprime-util generate --ut

That takes a few minutes to complete. You should see this at the end:

.. code-block:: text

    -- Build files have been written to: /home/jeff/workspace/Spacecraft/build-fprime-automatic-native-ut

Generate template testing files:

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

Create a test directory and move the test files into that folder:

.. code-block:: bash

    mkdir -p test/ut
    mv Test* test/ut

Add ``Tester.cpp`` and ``TestMain.cpp`` to the list of unit test source files in CMakeLists.txt:

.. code-block:: bash

    set(UT_SOURCE_FILES
      "${CMAKE_CURRENT_LIST_DIR}/Thruster.fpp"
      "${CMAKE_CURRENT_LIST_DIR}/test/ut/Tester.cpp"
      "${CMAKE_CURRENT_LIST_DIR}/test/ut/TestMain.cpp"
    )
    set(UT_AUTO_HELPERS ON)
    register_fprime_ut()

Build it:

.. code-block:: bash

    fprime-util build --ut

Unit tests are run using the ``check`` command:

.. code-block:: bash

    fprime-util check

That command is a wrapper around the executable file found here:

.. code-block:: bash

    ~/workspace/Spacecraft/build-fprime-automatic-native-ut/bin/Linux/Components_Thruster_ut_exe

But using the ``fprime-util check`` command you shouldn't ever need to run the executable file directly.
The ``fprime-util check`` command captures the output of the test and saves it in a log file,
and provides other features that we won't get into here.

The output should look like this:

.. code-block:: text

    test 1
        Start 1: Components_Thruster_ut_exe

    1: Test command: /home/jeff/workspace/Spacecraft/build-fprime-automatic-native-ut/bin/Linux/Components_Thruster_ut_exe
    1: Test timeout computed to be: 10000000
    1: [==========] Running 1 test from 1 test suite.
    1: [----------] Global test environment set-up.
    1: [----------] 1 test from Nominal
    1: [ RUN      ] Nominal.ToDo
    1: [       OK ] Nominal.ToDo (3 ms)
    1: [----------] 1 test from Nominal (3 ms total)
    1: 
    1: [----------] Global test environment tear-down
    1: [==========] 1 test from 1 test suite ran. (4 ms total)
    1: [  PASSED  ] 1 test.
    1/1 Test #1: Components_Thruster_ut_exe .......   Passed    0.05 sec

    100% tests passed, 0 tests failed out of 1

    Total Test time (real) =   0.06 sec
    [100%] Built target Components_Thruster_check

The important line is the summary indicating that ``100% tests passed``.

Currently the test program runs a dummy test that always passes.
We'll add some real tests in the next section.
