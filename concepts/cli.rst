Modelstore CLI commands
=======================

You can use modelstore (version > 0.0.71) from the command line. For example, to download a model, you can use:

.. code-block:: bash

    python -m modelstore download <domain> <model-id>

Modelstore figures out how to read from your storage by looking for specific environment variables.

.. list-table:: Storage environment variables
   :widths: 20 20 80
   :header-rows: 1

   * - Storage
     - MODEL_STORE_STORAGE
     - Other environment variables
   * - `AWS s3 <https://aws.amazon.com/s3/>`_
     - aws-s3
     - | MODEL_STORE_AWS_BUCKET
       | AWS_ACCESS_KEY_ID
       | AWS_SECRET_ACCESS_KEY
   * - `Azure Container <https://docs.microsoft.com/en-us/azure/container-instances/>`_
     - azure-container
     - | MODEL_STORE_AZURE_CONTAINER
       | AZURE_ACCOUNT_NAME
       | AZURE_ACCESS_KEY
       | AZURE_STORAGE_CONNECTION_STRING
   * - `Google Cloud Storage <https://cloud.google.com/storage>`_
     - google-cloud-storage
     - | MODEL_STORE_GCP_PROJECT
       | MODEL_STORE_GCP_BUCKET
   * - File system
     - filesystem
     - MODEL_STORE_ROOT

