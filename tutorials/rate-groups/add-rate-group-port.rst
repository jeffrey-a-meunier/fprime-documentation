Add a rate-group port
=====================

Edit the ``ThrusterController.fpp`` file.
Uncomment the ``Svc.Sched`` input port (but not the comment above it) and change its type to ``async``

Before

.. code-block:: text

    # @ Example port: receiving calls from the rate group
    # sync input port run: Svc.Sched

After

.. code-block:: text

    @ Port to receive calls from the rate group
    async input port run: Svc.Sched

Then execute this in the ``ThrusterController`` directory in order to generate the proper function handler for the ``run`` port.

.. code-block:: bash

  fprime-util impl

Modify ``ThrusterController.hpp``
---------------------------------
This is the ``run_handler`` declaration in the ``ThrusterController.hpp-template`` file.

.. code-block:: c++

    //! Handler implementation for run
    //!
    void run_handler(
        const NATIVE_INT_TYPE portNum, /*!< The port number*/
        NATIVE_UINT_TYPE context /*!< 
    The call order
    */
    );

Copy that declaration and paste it into the ``Thruster.hpp`` file.
You can copy it from this web page or from the template file. They are both the same.

Modify ``ThrusterController.cpp``
---------------------------------
This is the ``run_handler`` definition in the ``ThrusterController.cpp-template`` file.

.. code-block:: c++

  void Thruster ::
    run_handler(
        const NATIVE_INT_TYPE portNum,
        NATIVE_UINT_TYPE context
    )
  {
    // TODO
  }

Copy that definition and paste it into the ``ThrusterController.cpp`` file.
You can copy it from this web page or from the template file. They are both the same.

In the next section you'll fill in the body of the ``run_handler`` function.
