Quick Start
=======================================

This library's :code:`ModelStore` enables you to export trained ML models and store it to your choice of storage.

Create a model store instance
-----------------------------

:code:`modelstore` currently supports storing models to:

* Google Cloud buckets

The library assumes that you already have set up a Google Cloud project
and have `created a bucket <https://cloud.google.com/storage/docs/creating-buckets>`_.

To save your models, create a model store instance with::
        
   ms = ModelStore.from_gcloud(
      project_name="my-project",
      bucket_name="my-bucket",
   )

Export a model
--------------

The :code:`modelstore` library detects which types of machine learning libraries you have installed,
and automatically sets up functions that enable you to export a trained model.

All of the functions have the form :code:`ms.<library-name>.create_archive()`.

For example, to export a :code:`scikit-learn` model, use::

   archive = ms.sklearn.create_archive(model=my_model)

This function will create a file called :code:`artifacts.tar.gz` in your current
working directory, which contains the exported model.

To read more about the supported libraries, see: :doc:`../concepts/libraries`.

Upload the model to cloud storage
---------------------------------

Finally, to save your models into the Google Cloud Bucket, use :code:`upload()`::
        
   rsp = ms.upload(domain="example-model", archive)

When you upload a model, you need to specify a **domain**. This is the word we use
to group several models together. For example, let's assume you are training several
models to predict whether an email is spam. Setting :code:`domain="spam-detection"`
will store all of those models together.

To read more about how this library organises models, see :doc:`../concepts/modelstore`.