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

- A file system;
- Google Cloud storage buckets
- AWS s3 buckets
- Azure blob storage containers
- MinIO object storage

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

**AWS s3 Bucket**

.. code-block:: python

   model_store = ModelStore.from_aws_s3(
      bucket_name="my-bucket",
   )

**Azure Blob Storage**

.. code-block:: python

   model_store = ModelStore.from_azure(container_name="my-container-name")


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

