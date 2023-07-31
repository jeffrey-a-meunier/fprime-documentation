Troubleshooting: ``CMake Error... fpp tools not found``
=======================================================

Problem
-------

.. code-block:: text

    CMake Error at fprime/cmake/required.cmake:25 (message):
    -- Configuration header directory: /home/jeff/workspace/Spacecraft/fprime/config
    -- Configuring incomplete, errors occurred!
    fpp tools not found. Install with:
    See also "/home/jeff/workspace/Spacecraft/build-fprime-automatic-native/CMakeFiles/CMakeOutput.log".
        'pip install -r "/home/jeff/workspace/Spacecraft/fprime/requirements.txt"'
    Call Stack (most recent call first):
    fprime/cmake/FPrime.cmake:26 (include)
    CMakeLists.txt:11 (include)

Solution
--------

You probably forgot to source the Python virtual environment.

.. code-block:: bash

    jeff@ubuntu-vbox:~$ cd workspace/Spacecraft/
    jeff@ubuntu-vbox:~/workspace/Spacecraft$ source venv/bin/activate
    (venv) jeff@ubuntu-vbox:~/workspace/Spacecraft$ 
