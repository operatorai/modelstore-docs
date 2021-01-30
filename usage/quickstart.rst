Quick Start
=======================================

This library's :code:`ModelStore` enables you to export trained ML models and store it to your choice of storage.

Create a model store instance
-----------------------------

:code:`modelstore` currently supports storing models to:

* A directory in a local file system
* Google Cloud buckets: set up a Google Cloud project and have `create a cloud storage bucket <https://cloud.google.com/storage/docs/creating-buckets>`_.
* AWS S3 buckets: set up a project and `create an s3 bucket <https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html>`_.
* 🆕 A storage service that we manage for you. This requires you to have API keys.

To save your models, create a model store instance with one of the following::
   
   from modelstore import ModelStore

   # A local file system
   model_store = ModelStore.from_file_system(
      root="/path/to/directory",
   )

   # Google cloud bucket
   model_store = ModelStore.from_gcloud(
      project_name="my-project",
      bucket_name="my-bucket",
   )

   # AWS S3 bucket
   model_store = ModelStore.from_aws_s3(
      bucket_name="my-bucket",
   )

   # A managed storage service
   model_store = ModelStore.from_api_key(
      access_key_id="<your-access-key-id>",
      secret_access_key="<your-secret-access-key>"
   )

Upload a model to the model store
-----------------------------------

The :code:`modelstore` library has separate up functions to store models that were trained with different ML libraries, such as scikit-learn or tensorflow. They all follow the same pattern.

For example, to store a :code:`scikit-learn` model, use::

   model_store.sklearn.upload(domain="domain-name", model=my_model)

When you upload a model, you need to specify a **domain**. This is the string that
groups several models that are for the same end-usage together. For example, let's assume you are training several models to predict whether an email is spam. Setting :code:`domain="spam-detection"` will store all of those models together, and you will then be able to list and retrieve them all.

To read more about the supported libraries, see: :doc:`../concepts/libraries`.

To read more about how this library organises models, see :doc:`../concepts/modelstore`.

Download a model from the model store
-------------------------------------

To retrieve a model from your chosen storage, use :code:`download()`::

   file_path = model_store.download(
      local_path=".", # Where to download the model to
      domain="example-model", # The model's domain
      model_id="model-id"  # Optional; the ID of the specific model
   )

If you do not provide a :code:`model_id` parameter, the :code:`download()` function will default to the last model that was stored for the given domain.

