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
   * - `MinIO s3 storage <https://min.io/>`_
     - The name of an existing bucket and access credentials
     - `MinIO Example <https://github.com/operatorai/modelstore/blob/main/examples/examples-by-ml-library/modelstores.py#L44-L51>`_
   * - `Azure Container <https://docs.microsoft.com/en-us/azure/container-instances/>`_
     - The name of an existing container
     - `Azure Example <https://github.com/operatorai/modelstore/blob/main/examples/examples-by-ml-library/modelstores.py#L54-L63>`_
   * - `Google Cloud Storage <https://cloud.google.com/storage>`_
     - The name of an existing bucket
     - `Cloud Storage Example <https://github.com/operatorai/modelstore/blob/main/examples/examples-by-ml-library/modelstores.py#L66-L74>`_
   * - File system
     - A path
     - `File system Example <https://github.com/operatorai/modelstore/blob/main/examples/examples-by-ml-library/modelstores.py#L85>`_


File system storage
-------------------

The file system model storage assumes that (a) the root directory exists, and (b) the library user has permission to write to it. 

If you want to create the root directory if it does not exist, pass along the `create_directory=True` argument.

.. code-block:: python

    model_store = ModelStore.from_file_system(
      root_directory="/path/to/directory",
      create_directory=True,
    )

