Add a rate-group port
=====================

Uncomment the ``Svc.Sched`` input port and change its type to ``async``

Before

    # @ Example port: receiving calls from the rate group
    # sync input port run: Svc.Sched

After

    # @ Example port: receiving calls from the rate group
    async input port run: Svc.Sched

Modify ``Thruster.hpp``
-----------------------
This is the ``run_handler`` declaration in the ``Thruster.hpp-template`` file.

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

Modify ``Thruster.cpp``
-----------------------
This is the ``run_handler`` definition in the ``Thruster.cpp-template`` file.

.. code-block:: c++

  void Thruster ::
    run_handler(
        const NATIVE_INT_TYPE portNum,
        NATIVE_UINT_TYPE context
    )
  {
    // TODO
  }

Copy that definition and paste it into the ``Thruster.cpp`` file.
Replace the ``// TODO`` commend with this call to the telemetry function:

.. code-block:: c++

    this->tlmWrite_ExampleCounter(0); 