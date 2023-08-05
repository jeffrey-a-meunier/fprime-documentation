Tutorial 9: Unit tests
======================

It's important to write unit tests in order to test each component you've created.
It may seem strange to test code that you've written by writing even more code,
but the code you write in a unit test should never be complex.

In TDD (test-driven development) you write the unit tests first.
Typically you know precisely what the program needs to do before you start writing it.
In the case of an F' program, you should know precisely what each component needs to do before you create it.
In that case it's easy to write the unit tests that ensure that the components behave how they should.

For TDD the procedure would be this: (1) Write the tests, which all fail because the program hasn't been written yet.
Then (2) write the program and implement the features you need so that the tests pass.

In these tutorials, though, we've already created the components and got them working.
We didn't do any unit testing, but it's not like we didn't do any testing at all.
What we did was called *ad-hoc* testing. We ran the program and checked that each feature was working correctly.

Even after you get a program working it's still useful to write unit tests, though, for a number of reasons.

1. They are a form of documentation, showing future readers (which can even be yourself) the proper way to use the components in a program.
2. They protect from regressions.
   If someone makes changes to a component, by running the unit tests after making the changes you can be sure that the component still works correctly.

.. toctree::
   :maxdepth: 1
   :caption: Contents:
   :numbered:

   create-template-files
   make-it-fail
   what-to-test
   create-poweron-test
   complete-poweron-test
