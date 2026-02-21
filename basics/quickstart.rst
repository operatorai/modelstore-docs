Quick Start
=======================================

Install using pip, import in your code
--------------------------------------

The model store library is available via Pypi:

.. code-block:: bash

   pip install modelstore

In your code, import :code:`ModelStore` with:

.. code-block:: python

    from modelstore import ModelStore

Create a model store instance and point it to your storage
----------------------------------------------------------

The model store library supports storing models to blob storage across different cloud providers:

- A file system
- Google Cloud Storage buckets
- AWS S3 buckets
- Azure Blob Storage containers
- MinIO object storage
- Backblaze B2 buckets
- HDFS

Create a model store instance by using one of the following factory methods.

**File System Storage**

.. code-block:: python

    model_store = ModelStore.from_file_system(root_directory="/path/to/directory")

**Google Cloud Storage Bucket**

.. code-block:: python

   model_store = ModelStore.from_gcloud(
      project_name="my-project",
      bucket_name="my-bucket",
   )

**AWS S3 Bucket**

.. code-block:: python

   model_store = ModelStore.from_aws_s3(
      bucket_name="my-bucket",
   )

**Azure Blob Storage**

.. code-block:: python

   model_store = ModelStore.from_azure(container_name="my-container-name")

**MinIO Object Storage**

.. code-block:: python

   model_store = ModelStore.from_minio(
      endpoint="play.min.io",
      access_key="my-access-key",
      secret_key="my-secret-key",
      bucket_name="my-bucket",
   )

**Backblaze B2**

.. code-block:: python

   model_store = ModelStore.from_backblaze(
      bucket_name="my-bucket",
      key_id="my-key-id",
      application_key="my-application-key",
      endpoint="https://s3.us-west-004.backblazeb2.com",
      region="us-west-004",
   )

**HDFS**

.. code-block:: python

   model_store = ModelStore.from_hdfs(root_prefix="/path/to/hdfs/directory")


Upload a model to the model store
-----------------------------------

Model store has an :code:`upload()` function that will create an archive containing your model and upload it to your storage. 

Whenever you upload a model, you need to specify which domain it belongs to. A "domain" is a string that model store uses to group several models that are for the same end-usage together.

For example, let's say you've trained a scikit-learn model (which is stored in a variable called :code:`clf`) that is going to be used in a spam classifier domain.

To store the model, use:

.. code-block:: python

   meta_data = model_store.upload("spam-detection", model=clf)

The :code:`upload()` function returns a dictionary containing meta data about your model - including the id that has been assigned to it, which is in :code:`meta_data["model"]["model_id"]`.

Load a model from the model store
---------------------------------

Once a model has been stored, you can load it straight from storage back into memory using model store's :code:`load()` function. 

.. code-block:: python

   clf = model_store.load("spam-detection", model_id="abcd-abcd-abdc")

