Summary of changes
==================

#. Install tools
#. ``mkdir ~/workspace``
#. ``cd ~/workspace``
#. ``fprime-util new --project``, name ``Spacecraft``
#. ``cd Spacecraft``
#. ``source venv/bin/activate``
#. ``fprime util --generate``
#. ``fprime-util build -j4``
#. ``fprime-util new --deployment``, name ``SpacecraftDeployment``
#. ``cd SpacecraftDeployment``
#. ``fprime-util generate``
#. ``fprime-util build -j4``
#. ``fprime-gds --gui-addr 0.0.0.0``
