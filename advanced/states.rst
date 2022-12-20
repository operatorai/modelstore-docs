Controlling model states
========================

This library enables you to control models by setting their state. For example, you may want to set a model to have state "production." You can then query the model store for models by state, and change model states.

The examples below assume you have created a model store instance already:

.. code-block:: python

    from modelstore.model_store import ModelStore

    model_store = ModelStore.from_aws_s3(bucket_name)

Create a state
--------------

Before doing anything with a model state, you need to create it. This is a one-time operation.


.. code-block:: python

    production_state = "production"

    model_store.create_model_state(production_state)

Set and unset a model's state
-----------------------------

Once a state has been created, you can add a model to a state. You can add a model to more than one state, and you can add more than one model to a state.

.. code-block:: python

    model_domain = "my-domain"
    model_id = "my-model-id"
    production_state = "production"

    model_store.set_model_state(model_domain, model_id, state_name)

To unset a model's state, you can use:

.. code-block:: python

    model_store.remove_model_state(model_domain, model_id, state_name)

Find models by state
--------------------

After setting the state of one or more models, you can find them by adding the state name to the list versions function:

.. code-block:: python

    model_ids = modelstore.list_versions(
        model_domain,
        state_name=production_state
    )
