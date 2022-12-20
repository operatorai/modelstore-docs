Additional download functionality
=================================

Providing read-only access
--------------------------

The AWS s3, Google GCS, Azure Containers storage types assume that (a) the bucket/container exists, and (b) the library user has both read and write permissions.

As of 0.0.74, modelstore also supports read-only access to public Google Cloud Storage buckets.

Download a model from the model store
-------------------------------------

If you would rather download the model, and not load it into memory, you can use model store's :code:`download()` function. 

.. code-block:: python

   file_path = model_store.download(
      local_path=".", # Where to download the model to
      domain="example-model", # The model's domain
      model_id="model-id"  # Optional; the ID of the specific model
   )