Make it fail
============

Examine the test
----------------
If you view the contents of the ``TestMain.cpp`` file you will see this test definition:

.. code-block:: c++

    TEST(Nominal, ToDo) {
        Components::Tester tester;
        tester.toDo();
    }

That test invokes the ``toDo()`` test function, which is in the ``Tester.cpp`` file.
This is what that function looks like:

.. code-block:: c++

  void Tester ::
    toDo()
  {
    // TODO
  }

Force a failure
---------------
Currently that test function does nothing. Replace the ``TODO`` comment with this line:

.. code-block:: c++

    ASSERT_TRUE(false);

That assertion check should fail because it's asserting that the argument is true, when in fact it's ``false``.

Make sure that the function now looks like this:

.. code-block:: c++

  void Tester ::
    toDo()
  {
    ASSERT_TRUE(false);
  }

Save the file.

Run the test
------------
Enter this command:

.. code-block:: bash

    fprime-util check

Notice that the output now shows a single test failure:

.. code-block:: text

    test 1
        Start 1: Components_Thruster_ut_exe

    1: Test command: /home/jeff/workspace/Spacecraft/build-fprime-automatic-native-ut/bin/Linux/Components_Thruster_ut_exe
    1: Test timeout computed to be: 10000000
    1: [==========] Running 1 test from 1 test suite.
    1: [----------] Global test environment set-up.
    1: [----------] 1 test from Nominal
    1: [ RUN      ] Nominal.ToDo
    1: /home/jeff/workspace/Spacecraft/Components/Thruster/test/ut/Tester.cpp:37: Failure
    1: Value of: false
    1:   Actual: false
    1: Expected: true
    1: [  FAILED  ] Nominal.ToDo (8 ms)
    1: [----------] 1 test from Nominal (8 ms total)
    1: 
    1: [----------] Global test environment tear-down
    1: [==========] 1 test from 1 test suite ran. (9 ms total)
    1: [  PASSED  ] 0 tests.
    1: [  FAILED  ] 1 test, listed below:
    1: [  FAILED  ] Nominal.ToDo
    1: 
    1:  1 FAILED TEST

It indicates the source line, Tester.cpp:37, and what it expected to happen:

.. code-block:: text

    1: Value of: false
    1:   Actual: false
    1: Expected: true

Now all that's left is to fix the program so that the test passes.

Edit the ``toDo`` function in Tester.cpp and change ``false`` to ``true``:

.. code-block:: c++

  void Tester ::
    toDo()
  {
    ASSERT_TRUE(true);
  }

Run the test again and observe the output.

.. code-block:: text

    fprime-util check

    1: [==========] Running 1 test from 1 test suite.
    1: [----------] Global test environment set-up.
    1: [----------] 1 test from Nominal
    1: [ RUN      ] Nominal.ToDo
    1: [       OK ] Nominal.ToDo (3 ms)
    1: [----------] 1 test from Nominal (3 ms total)
    1: 
    1: [----------] Global test environment tear-down
    1: [==========] 1 test from 1 test suite ran. (3 ms total)
    1: [  PASSED  ] 1 test.

    100% tests passed, 0 tests failed out of 1
