Additional functionality
========================

Deleting Models
---------------

Deleting a model removes the files from the registry. If you query for a model that has been deleted, a ModelDeletedException is raised.


.. code-block:: python

    # Delete a model
    model_store.delete_model("my-domain", "my-model", skip_prompt=True)

    # Will raise a ModelDeletedException
    meta_data = model_store.get_model_info("my-domain", "my-model")

