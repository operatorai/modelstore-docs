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
     - `AWS Example <https://github.com/operatorai/modelstore/blob/main/examples/examples-by-ml-library/modelstores.py#L36-L41>`_
   * - `Backblaze B2 <https://www.backblaze.com/cloud-storage>`_
     - The name of an existing bucket, key ID, application key, endpoint, and region
     - n/a
   * - `MinIO s3 storage <https://min.io/>`_
     - The name of an existing bucket and access credentials
     - `MinIO Example <https://github.com/operatorai/modelstore/blob/main/examples/examples-by-ml-library/modelstores.py#L44-L51>`_
   * - `Azure Container <https://docs.microsoft.com/en-us/azure/container-instances/>`_
     - The name of an existing container
     - `Azure Example <https://github.com/operatorai/modelstore/blob/main/examples/examples-by-ml-library/modelstores.py#L54-L63>`_
   * - `Google Cloud Storage <https://cloud.google.com/storage>`_
     - The name of an existing bucket
     - `Cloud Storage Example <https://github.com/operatorai/modelstore/blob/main/examples/examples-by-ml-library/modelstores.py#L66-L74>`_
   * - HDFS
     - A root prefix path; requires ``pydoop``
     - n/a
   * - File system
     - A path
     - `File system Example <https://github.com/operatorai/modelstore/blob/main/examples/examples-by-ml-library/modelstores.py#L85>`_


Backblaze B2 storage
--------------------

Backblaze B2 uses the S3-compatible API (via ``boto3``). The bucket must already exist.

.. code-block:: python

    model_store = ModelStore.from_backblaze(
      bucket_name="my-bucket",
      key_id="my-key-id",
      application_key="my-application-key",
      endpoint="https://s3.us-west-004.backblazeb2.com",
      region="us-west-004",
    )

HDFS storage
------------

HDFS storage requires ``pydoop`` to be installed.

.. code-block:: python

    model_store = ModelStore.from_hdfs(
      root_prefix="/path/to/hdfs/directory",
    )

File system storage
-------------------

The file system model storage assumes that (a) the root directory exists, and (b) the library user has permission to write to it.

If you want to create the root directory if it does not exist, pass along the `create_directory=True` argument.

.. code-block:: python

    model_store = ModelStore.from_file_system(
      root_directory="/path/to/directory",
      create_directory=True,
    )

