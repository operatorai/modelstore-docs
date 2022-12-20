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

Download a model from the model store
-------------------------------------

If you would rather download the model, and not load it into memory, you can use model store's :code:`download()` function. 

.. code-block:: python

   file_path = model_store.download(
      local_path=".", # Where to download the model to
      domain="example-model", # The model's domain
      model_id="model-id"  # Optional; the ID of the specific model
   )