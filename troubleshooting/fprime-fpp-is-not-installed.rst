Troubleshooting: ``[WARNING] fprime-fpp is not installed``
==========================================================

Problem
-------

.. code-block:: text

    /usr/lib/python3/dist-packages/pkg_resources/__init__.py:116: PkgResourcesDeprecationWarning: 1.1build1 is an invalid version and will not be supported in a future release
    warnings.warn(
    [WARNING] fprime-fpp is not installed
    [WARNING] fprime-tools has unexpected version. Expected: 3.2.0 found 3.2.1
    [WARNING] fprime-gds is not installed

Solution
--------
You probably forgot to source the Python virtual environment.

.. code-block:: bash

    jeff@ubuntu-vbox:~$ cd workspace/Spacecraft/
    jeff@ubuntu-vbox:~/workspace/Spacecraft$ source venv/bin/activate
    (venv) jeff@ubuntu-vbox:~/workspace/Spacecraft$ 
