Modelstore CLI commands
=======================

You can use modelstore (version > 0.0.71) from the command line to upload and download models:

.. code-block:: bash
    
    # To upload a model
    python -m modelstore upload <domain> </path/to/file>
    
    # To download a model
    python -m modelstore download <domain> <model-id>


Modelstore figures out how to read from your storage by looking for specific environment variables.

Your environment needs to define (1) a value for MODEL_STORE_STORAGE which tells modelstore what type of storage you are using, and (2) values that depend on the specific type of storage that you are using.

All of these are summarised in the table below:

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

