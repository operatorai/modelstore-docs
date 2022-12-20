Key Concepts
============

The :code:`modelstore` library is built around a few key concepts.

Model Archive
-------------

When you upload a model, an :code:`artifacts.tar.gz` file is created and then uploaded to your storage. This archive contains:

1. Files that are dumps from your model,
2. A :code:`"python-info.json"` file that enumerates the version of the Python library of the model you are exporting.


Model Meta-data
---------------

The :code:`upload()` function returns a dictionary containing meta-data about the model, which includes an id for your model.

The meta-data includes:

* A unique id for your model;
* Details about where the model is being uploaded to (the bucket and prefix);
* The Python runtime that was used (e.g., "python:3.7.0")
* The user `who ran the upload <https://docs.python.org/3/library/getpass.html#getpass.getuser>`_.
* Versions for the Python library and key dependencies.

Domains
-------

A **domain** is how :code:`modelstore` denotes a group of models, that are all intended for the same end-usage. When you upload a model to the store, you will add it to a domain.

The model store library then allows you to list the models that are in a domain and retrieve specific models (e.g., the latest one).

Under the hood, a domain is just a string, so it is up to you how you would like
to use it.

Model State
-----------

A **model state** is a tag that you can use to control the lifecycle of a model in a given domain.

For example, you may want to have some models tagged as being in state "production" or state "shadow." You can achieve this by creating a state and then setting a model's state.

Under the hood, a model state name is just a string, so it is up to you how you would like to use it.


Storage
-------

When you pick a backend that stores data in files (e.g., Cloud Storage Buckets), the files are stored with a pre-defined structure.

The top-level, **root** prefix that this library hard-codes is :code:`operatorai-model-store`.

When you create and upload a model archive, this library will upload three files to different places in the bucket.

1.  **The artifacts archive** will be uploaded to: :code:`root/<domain>/<datetime>/archive.tar.gz`, where the datetime has the form :code:`"%Y/%m/%d/%H:%M:%S"` - denoting the time when the model was uploaded.
2. The library creates a dictionary of **meta-data** about your model. This will be uploaded to :code:`root/<domain>/versions/<model-id>.json`.
3. This same meta-data is also stored in :code:`root/<domain>/latest.json`, which tracks the _last_ model that was uploaded to the model store.
