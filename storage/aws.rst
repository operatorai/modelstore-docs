Storing models in AWS s3
========================

This page describes the steps you need to take to store models in s3.

Before you start, you will need to **create the s3 bucket you want to use**. The modelstore library does not currently create s3 buckets and assumes they exist already. To do this, you can follow the `creating a bucket <https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html>`_ AWS documentation.

Next, install modelstore and boto3 in your Python environment:

.. code-block:: bash

    pip install modelstore boto3

If you have not done this before, you will need to set up the AWS authentication credentials by following the `boto3 configuration guide <https://boto3.amazonaws.com/v1/documentation/api/latest/guide/quickstart.html#configuration>`_.

And you can then create a model store instance and point it to your bucket:

.. code-block:: python

    from modelstore import ModelStore

    model_store = ModelStore.from_aws_s3("my-bucket")

The remainder of this page describes some common errors you may run into. If you need further support, please `create an issue on Github <https://github.com/operatorai/modelstore/issues>`_.

ModuleNotFoundError: boto3 is not installed
-------------------------------------------

The model store library works with several different types of storage, and therefore does not install all of their libraries. If you see a :code:`ModuleNotFoundError`, then you need to install `boto3 <https://boto3.amazonaws.com/v1/documentation/api/latest/index.html>`_.

.. code-block:: bash

    pip install boto3

The current version of modelstore requires :code:`boto3>=1.6.16,<1.8`.

botocore.exceptions.NoCredentialsError: Unable to locate credentials
--------------------------------------------------------------------

You will need to set up the AWS authentication credentials. As `this documentation page <https://boto3.amazonaws.com/v1/documentation/api/latest/guide/configuration.html>`_ describes, boto3 "looks at various configuration locations until it finds configuration values." 

To start, follow the `AWS documentation <https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys>`_ to get your access key and secret access key values. There are then two approaches you can use here.

Option 1: run :code:`aws configure` by following the `boto3 configuration guide <https://boto3.amazonaws.com/v1/documentation/api/latest/guide/quickstart.html#configuration>`_.

.. code-block:: bash

    ❯ aws configure
    AWS Access Key ID [None]: my-access-key
    AWS Secret Access Key [None]: my-secret-access-key

Option 2: set environment variables. 

.. code-block:: bash

    export AWS_ACCESS_KEY_ID="my-access-key"
    export AWS_SECRET_ACCESS_KEY="my-secret-access-key"

botocore.exceptions.ParamValidationError: Parameter validation failed
---------------------------------------------------------------------

You'll see this error if you are passing a :code:`bucket_name` that boto3 cannot parse. Note: you do not need to include the "s3://" in the bucket name.

.. code-block:: python

    >>> model_store = ModelStore.from_aws_s3("s3://my-bucket-name")
    [...]
    botocore.exceptions.ParamValidationError: Parameter validation failed:
    Invalid bucket name "s3://my-bucket-name": Bucket name must match the regex "^[a-zA-Z0-9.\-_]{1,255}$" or be an ARN matching the regex "^arn:(aws).*:(s3|s3-object-lambda):[a-z\-0-9]*:[0-9]{12}:accesspoint[/:][a-zA-Z0-9\-.]{1,63}$|^arn:(aws).*:s3-outposts:[a-z\-0-9]+:[0-9]{12}:outpost[/:][a-zA-Z0-9\-]{1,63}[/:]accesspoint[/:][a-zA-Z0-9\-]{1,63}$"

Exception: Failed to set up the AWSStorage storage
--------------------------------------------------

This exception is raised if modelstore can't read from the bucket you are pointing it to. With logging enabled, you will see this line when you try to create a model store instance:

.. code-block:: python

    >>> model_store = ModelStore.from_aws_s3("my-bucket-name")
    Unable to access bucket: <bucket-name>

    [...]
    Exception: Failed to set up the AWSStorage storage

To resolve this, you can check:

1. Does the bucket exist? If not, you can follow the `creating a bucket <https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html>`_ AWS documentation.
2. Is there a typo in the :code:`bucket_name` variable?

botocore.exceptions.EndpointConnectionError: Could not connect to the endpoint URL
----------------------------------------------------------------------------------

This exception is raised if modelstore can't connect to the s3 bucket. One way this happens is if you specify a region that is not a known value. The full list of regions is available on `this AWS documentation page <https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.RegionsAndAvailabilityZones.html>`_.

For example, if you use a region name, you'll see an error:

.. code-block:: python

    >>> model_store = ModelStore.from_aws_s3(bucket_name=os.environ["AWS_BUCKET_NAME"], region="Frankfurt")
    >>> model_store.list_domains()
    [...]
    raise EndpointConnectionError(endpoint_url=request.url, error=e)
    botocore.exceptions.EndpointConnectionError: Could not connect to the endpoint URL: "https://operator-ai-modelstore-direct.s3.Frankfurt.amazonaws.com/?list-type=2&prefix=operatorai-model-store%2Fdomains&encoding-type=url"

But if you use the region code, it should not error:

.. code-block:: python

    >>> model_store = ModelStore.from_aws_s3(bucket_name=os.environ["AWS_BUCKET_NAME"], region="eu-central-1")
    >>> model_store.list_domains()
    ['diabetes-boosting-demo']
