The Model Store Structure
=======================================

This library's model store interacts with a backend of your chosing. The library currently supports:

* A local directory
* `Google Cloud Storage <https://cloud.google.com/storage>`_
* `AWS S3 Buckets <https://aws.amazon.com/s3/>`_

This library stores models in cloud buckets using a pre-defined structure.

Model Archive & Meta Data
-------------------------

When you use :code:`create_archive()`, an :code:`artifacts.tar.gz` file is created
in the current working directory. This archive contains:

1. Any files that were dumped from your model,
2. A :code:`"python-info.json"` file that enumerates the version of the Python library of the model you are exporting.
3. Files containing any additional data you want to add to the archive.

When you :code:`upload()` a model archive, this library uploads the archive to cloud
storage, and creates and returns a dictionary containing meta-data about the model.

The meta-data includes:

* A unique `UUID4` for your model;
* Details about where the model is being uploaded to (the bucket and prefix);
* The Python runtime that was used (e.g., "python:3.7.0")
* The user `who ran the training <https://docs.python.org/3/library/getpass.html#getpass.getuser>`_.
* Versions for the Python library and key dependencies.

Model Domains
-------------

A **domain** is the word we use to group models, that are all intended for the
same end-usage, together.

Under the hood, this is just a string, so it is up to you how you would like
to use it; it is required because this library stores models by domain.

File Storage Structure
----------------------

When you pick a backend that stores data in files (e.g., Cloud Storage Buckets), the files
are stored with a pre-defined structure.

The top-level, **root** prefix that this library hard-codes is :code:`operatorai-model-store`.

When you create and upload a model archive, this library will upload three files
to different places in the bucket.

1.  **The artifacts archive** will be uploaded to: :code:`root/<domain>/<datetime>/archive.tar.gz`, where
the datetime has the form :code:`"%Y/%m/%d/%H:%M:%S"` - denoting the time when the model was
uploaded.
2. The library creates a dictionary of **meta-data** about your model. This will be uploaded
to :code:`root/<domain>/versions/<model-id>.json`.
3. This same meta-data is also stored in :code:`root/<domain>/latest.json`, which tracks the _last_ model that was uploaded to the
model store.

Example
^^^^^^^

Let's imagine you're training a text classifier to detect whether some customer 
text is about "refunds." 

Over time, you may end up re-training this classifier several times, with newer data
or different models types; however, you still  need a way to denote that all of these
models were about detecting refund requests.

In this case, you could set the :code:`domain="customer-refunds"`.

Models that are exported in this domain will be stored to::

    <root>/<domain>/<time/of/upload>/artifacts.tar.gz
    operatorai-model-store/customer-refunds/2020/08/30/23:29:28/artifacts.tar.gz
