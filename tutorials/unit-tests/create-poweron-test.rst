Create a PowerOn test
=====================

Now we'll create a test for the PowerOn command.

Rename the ``toDo`` test
------------------------
Edit the ``test/ut/TestMain.cpp`` file and replace this test:

.. code-block:: c++

    TEST(Nominal, ToDo) {
        Components::Tester tester;
        tester.toDo();
    }

with this one:

.. code-block:: c++

    TEST(Nominal, TestPowerOn) {
        Components::Tester tester;
        tester.testPowerOn();
    }

The name of the test is now ``TestPowerOn`` and it now calls the function ``tester.testPowerOn()``.

Rename the test function
------------------------
Edit the ``test/ut/Tester.hpp`` file and change this function declaration:

.. code-block:: c++

    //! To do
    //!
    void toDo();

to this:

.. code-block:: c++

    //! Test PowerOn command handler
    //!
    void testPowerOn();

Edit the ``test/ut/Tester.cpp`` file and rename the ``toDo`` function to ``testPowerOn``.

.. code-block:: c++

  void Tester ::
    testPowerOn()
  {
    ASSERT_TRUE(true);
  }

Finally in the ``test/ut/TestMain.cpp`` file, the ``toDo`` function call must be changed to ``testPowerOn``:abbr:

.. code-block:: c++

    TEST(Nominal, ToDo) {
        Components::Tester tester;
        tester.testPowerOn();
    }

Run the test again to be sure it still works.

.. code-block:: bash

    fprime-util check

    100% tests passed, 0 tests failed out of 1
