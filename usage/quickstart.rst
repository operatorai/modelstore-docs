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

   ms = ModelStore.from_gcloud(
      project_name="my-project",
      bucket_name="my-bucket",
   )

For AWS, use :code:`ModelStore.from_aws_s3(bucket_name="<s3-bucket>")`

For your local file system, use :code:`ModelStore.from_file_system(root="<path>")`

Export a model
--------------

The :code:`modelstore` library detects which types of machine learning libraries you have installed, and automatically sets up functions that enable you to export a trained model.

All of the functions have the form :code:`ms.<library-name>.create_archive()`.

For example, to export a :code:`scikit-learn` model, use::

   archive = ms.sklearn.create_archive(model=my_model)

This function will create a file called :code:`artifacts.tar.gz` in your current
working directory, which contains the exported model.

To read more about the supported libraries, see: :doc:`../concepts/libraries`.

Upload the model to cloud storage
---------------------------------

To save your models into your chosen storage, use :code:`upload()`::
        
   rsp = ms.upload(domain="example-model", archive)

When you upload a model, you need to specify a **domain**. This is the word we use
to group several models together. For example, let's assume you are training several
models to predict whether an email is spam. Setting :code:`domain="spam-detection"`
will store all of those models together.

To read more about how this library organises models, see :doc:`../concepts/modelstore`.

Download a model from cloud storage
-----------------------------------

To retrieve a model from your chosen storage, use :code:`download()`::
        
   file_path = ms.download(local_path=".", domain="example-model", model_id="model-id")

This function will download the :code:`artifacts.tar.gz` that you stored, extract all the files from it, and remove the tar file.

If you do not provide a :code:`model_id` parameter, the :code:`download()` function will default to the last model that was stored for the given domain.

