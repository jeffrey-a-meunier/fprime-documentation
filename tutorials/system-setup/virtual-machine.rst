Install a virtual machine
=========================
Users have the option of installing F' and all its associated tools natively,
or installing everything in a virtual machine.
Users not wishing to use a virtual machine may skip ahead to the next section.

Windows users:
    * Windows Subsystem for Linux is an ideal option.
    * VirtualBox is a good option.

Intel Mac users:
    * VirtualBox is your best option.
    * Parallels is a good option, but you must pay to use it after a 30-day trial period.

M1/M2 Mac users:
    * As of this writing (mid-2023), VirtualBox does not work well.
    * Parallels is your only option, but you can still choose to install everything natively instead of using a VM.

Linux users
    * You can install a VM or install everything natively.
    * Running Linux in a VirtualBox VM is a good way to isolate changes you make to your system.

Install Windows Subsystem for Linux
-----------------------------------
TODO

VirtualBox
----------

Install VirtualBox
~~~~~~~~~~~~~~~~~~
https://www.virtualbox.org

Install Linux in a VM
~~~~~~~~~~~~~~~~~~~~~
Before you download Linux you should decide how you will want to use it.

If you want to work solely within the Linux VM, then I recommend installing a *desktop* version of Linux.
This will give you a graphical desktop and allow you to run Visual Studio Code and also use a web browser.

Nevertheless I will show you how to develop F' projects using the VM as a remote server,
so you will run a text editor, terminal window, and web browser on your host computer (i.e., in Windows or Mac OS).

Considerations:

* If you want to do all your development within Linux, then choose a *desktop* version of Linux.
* If you know you will not need a graphical desktop in Linux, then you can choose a *server* version of Linux.
   * This could save several gigabytes of space on your computer, and the download will be faster.
   * Note that there will be no way to run any graphical applications inside Linux. You will not need them for this series of tutorials.
* Any version of Linux from 2020 or newer should work fine.

I recommend using Ubuntu Linux, but you can use any distribution you like.

* https://ubuntu.com/download/desktop

Start VirtualBox and create a new Linux VM
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
VirtualBox does a good job of setting the default values to run the VM on your computer.

Configure the VM
~~~~~~~~~~~~~~~~

.. code-block:: bash

    sudo update
    sudo upgrade

Install OpenSSH server
~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    sudo apt install openssh-server

Install Guest Additions
~~~~~~~~~~~~~~~~~~~~~~~
It's under the **Devices** menu for the VM.

.. code-block:: bash

    sudo apt install gcc make perl

Forward ports
~~~~~~~~~~~~~

SSH 2222 22
GDS 5000 5000

Ensure Python is installed
--------------------------
Be sure you have a recent distribution of Python 3 installed.
You can check the version of Python by entering this command in the terminal window:

.. code-block:: bash

   python3 --version

It should respond with information similar to this:

.. code-block:: text

   Python 3.10.6

If it complains that it can't find python3, then if you're running Ubuntu Linux, then the simplest way to install it is by entering this command:

.. code-block:: bash

   sudo apt install python3

Otherwise, you can obtain the latest version of Python here: https://www.python.org

Login credentials
-----------------
Take careful note of your login credentials for Linux: your user name and password.
You will need them to log back into Linux,
and you will also need them if you decide to work *host-to-guest*, as I describe in the next section.
