Retrieving model and domain information
=======================================

This library enables you to query your model registry programmatically.

The examples below assume you have created a model store instance already:

.. code-block:: python

    from modelstore.model_store import ModelStore

    model_store = ModelStore.from_aws_s3(bucket_name)

Model domains
--------------

Models are uploaded into domains: a domain is created when you upload your first model to it. You can list all of the existing domains and get information about a specific domain with:


.. code-block:: python

    model_domains = model_store.list_domains()

    meta_data = model_store.get_domain("my-domain")


Model states
------------

Model states are tags that can be used to control the lifecycle of models in a domain. To see the list of model states that have been created, use:


.. code-block:: python

    model_states = model_store.list_model_states()

Note: there are some reserved states that modelstore uses to, for example, keep track of model IDs that have been deleted. 


Model versions
--------------

Models are uploaded into domains: a domain is created when you upload your first model to it. You can list all of the existing domains and get information about a specific domain with:


.. code-block:: python

    # List all models
    model_ids = model_store.list_versions("my-domain")

    # List models with a given state
    prod_model_ids = model_store.list_versions("my-domain", state_name="production")


Models
------

The main thing you can do with a model is download or load it back. You can also retrieve information about a specific model, and delete models from the registry.


.. code-block:: python

    # Get information about a specific model
    meta_data = model_store.get_model_info("my-domain", "my-model")

