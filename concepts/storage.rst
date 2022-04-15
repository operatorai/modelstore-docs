Supported Storage Types
=======================

This library currently supports several places where you can save your models. You specify the storage type when you create a ModelStore instance:

.. list-table:: Supported storage types
   :widths: 25 50 25
   :header-rows: 1

   * - Storage
     - Requires
     - Example code
   * - `AWS s3 <https://aws.amazon.com/s3/>`_
     - The name of an existing s3 bucket
     - `AWS Example <https://github.com/operatorai/modelstore/blob/main/examples/examples-by-storage/modelstores.py#L17-L21>`_
   * - `Azure Container <https://docs.microsoft.com/en-us/azure/container-instances/>`_
     - The name of an existing container
     - `Azure Example <https://github.com/operatorai/modelstore/blob/main/examples/examples-by-storage/modelstores.py#L24-L31>`_
   * - `Google Cloud Storage <https://cloud.google.com/storage>`_
     - The name of an existing bucket
     - `Cloud Storage Example <https://github.com/operatorai/modelstore/blob/main/examples/examples-by-storage/modelstores.py#L34-L41>`_
   * - File system
     - A path
     - `File system Example <https://github.com/operatorai/modelstore/blob/main/examples/examples-by-storage/modelstores.py#L44-L49>`_


File system storage
-------------------

The file system model storage assumes that (a) the root directory exists, and (b) the library user has permission to write to it. 

If you want to create the root directory if it does not exist, pass along the `create_directory=True` argument.

.. code-block:: python

    model_store = ModelStore.from_file_system(
      root_directory="/path/to/directory",
      create_directory=True,
    )


Read-only access
----------------

The three cloud storage types (AWS s3, Google GCS, Azure Containers) assume that (a) the bucket/container exists, and (b) the library user has both read and write permissions.

As of 0.0.74, modelstore also supports read-only access to public Google Cloud Storage buckets.

