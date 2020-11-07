Quick Start
=======================================

This library's :code:`ModelStore` enables you to export trained ML models and store it to your choice of storage.

Create a model store instance
-----------------------------

:code:`modelstore` currently supports storing models to:

* Your local file system
* Google Cloud buckets: The library assumes that you already have set up a Google Cloud project and have `created a cloud storage bucket <https://cloud.google.com/storage/docs/creating-buckets>`_.
* AWS S3 buckets: As above, this library assumes that you have already set up a project and have `created an s3 bucket <https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html>`_.

To save your models, create a model store instance with::
   
   from modelstore import ModelStore

   model_store = ModelStore.from_gcloud(
      project_name="my-project",
      bucket_name="my-bucket",
   )

For AWS, use :code:`ModelStore.from_aws_s3(bucket_name="<s3-bucket>")`

For your local file system, use :code:`ModelStore.from_file_system(root="<path>")`

Upload the model to the model store
-----------------------------------

The :code:`modelstore` library detects which types of machine learning libraries you have installed, and automatically sets up functions that enable you to export a trained model.

When you upload a model, you need to specify a **domain**. This is the word we use
to group several models that are for the same end-usage together. For example, let's assume you are training several models to predict whether an email is spam. Setting :code:`domain="spam-detection"` will store all of those models together.

All of the functions have the form :code:`ms.<library-name>.upload()`.

For example, to export a :code:`scikit-learn` model, use::

   metadata = model_store.sklearn.upload(domain="domain-name", model=my_model)

This function will create a file called :code:`artifacts.tar.gz` in your current
working directory, which contains the exported model and some meta data, and uploads it to the bucket or location you have selected.

To read more about the supported libraries, see: :doc:`../concepts/libraries`.

To read more about how this library organises models, see :doc:`../concepts/modelstore`.

Download a model from the model store
-------------------------------------

To retrieve a model from your chosen storage, use :code:`download()`::

   file_path = model_store.download(
      local_path=".",
      domain="example-model",
      model_id="model-id"  # Optional
   )

This function will download an :code:`artifacts.tar.gz` that was stored, extract all the files from it, and remove the tar file.

If you do not provide a :code:`model_id` parameter, the :code:`download()` function will default to the last model that was stored for the given domain.

