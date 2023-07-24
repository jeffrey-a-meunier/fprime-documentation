Add a command
=============

I want to add one additional command to the component that will allow the GDS to query the thruster for its power status.
The component will respond with an event indicating whether its power is on or off.
The event will be created in the next section, along with several other events.

Modify the ``.fpp`` file
------------------------
Start a text editor and edit the ``Thruster.fpp`` file in the Components/Thruster directory.

Add this command to the ``Thruster.fpp`` file.

.. code-block:: text

    @ Command to fetch the status of the thruster power
    async command PowerStatus()
