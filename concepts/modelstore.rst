The Model Store Structure
=======================================

This library's model store interacts with a backend of your chosing. The library currently supports:

* A local file store
* Google Cloud Storage
* AWS S3 Buckets
* Azure Blob Containers

If you do not want to manage your own storage system, we also have a hosted storage that you can use with an API key.

Model Domains
-------------

A **domain** is how model store denotes a group of models, that are all intended for the same end-usage. When you upload a model to the store, you will add it to a domain.

The model store library then allows you to list the models that are in a domain and retrieve specific models (e.g., the latest one).

Under the hood, a domain is just a string, so it is up to you how you would like
to use it.

Model Archive & Meta Data
-------------------------

When you upload a model, an :code:`artifacts.tar.gz` file is created and then uploaded to the storage of your choice. This archive contains:

1. Any files that were dumped from your model,
2. A :code:`"python-info.json"` file that enumerates the version of the Python library of the model you are exporting.

The :code:`upload()` function returns a dictionary containing meta-data about the model.

The meta-data includes:

* A unique `UUID4` for your model;
* Details about where the model is being uploaded to (the bucket and prefix);
* The Python runtime that was used (e.g., "python:3.7.0")
* The user `who ran the training <https://docs.python.org/3/library/getpass.html#getpass.getuser>`_.
* Versions for the Python library and key dependencies.

File Storage Structure
----------------------

When you pick a backend that stores data in files (e.g., Cloud Storage Buckets), the files
are stored with a pre-defined structure.

The top-level, **root** prefix that this library hard-codes is :code:`operatorai-model-store`.

When you create and upload a model archive, this library will upload three files
to different places in the bucket.

1.  **The artifacts archive** will be uploaded to: :code:`root/<domain>/<datetime>/archive.tar.gz`, where the datetime has the form :code:`"%Y/%m/%d/%H:%M:%S"` - denoting the time when the model was uploaded.
2. The library creates a dictionary of **meta-data** about your model. This will be uploaded to :code:`root/<domain>/versions/<model-id>.json`.
3. This same meta-data is also stored in :code:`root/<domain>/latest.json`, which tracks the _last_ model that was uploaded to the model store.

Example
^^^^^^^

Let's imagine you're training a text classifier to detect whether some customer 
text is about "refunds." 

Over time, you may end up re-training this classifier several times, with newer data or different models types; however, you still  need a way to denote that all of these models were about detecting refund requests.

In this case, you could set the :code:`domain="customer-refunds"`.

Models that are exported in this domain will be stored to::

    <root>/<domain>/<time/of/upload>/artifacts.tar.gz
    operatorai-model-store/customer-refunds/2020/08/30/23:29:28/artifacts.tar.gz
