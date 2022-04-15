Supported Machine Learning Libraries
=======================================

This library currently supports several different machine learning libraries. To save models trained with them, you should use the upload function:

.. code-block:: python

    model_store.upload("domain", <kwargs>)

.. list-table:: Supported machine learning libraries
   :widths: 25 50 25
   :header-rows: 1

   * - Library
     - Required kwargs
     - Example code
   * - `Annoy <https://github.com/spotify/annoy>`_
     - model
     - `Annoy Example <https://github.com/operatorai/modelstore/blob/main/examples/examples-by-ml-library/libraries/annoy_example.py>`_
   * - `CatBoost <https://catboost.ai/>`_
     - model, pool (for classification)
     - `Catboost Example <https://github.com/operatorai/modelstore/blob/main/examples/examples-by-ml-library/libraries/catboost_example.py>`_
   * - `FastAI <https://github.com/fastai/fastai/>`_
     - learner
     - `FastAI Example <https://github.com/operatorai/modelstore/blob/main/examples/examples-by-ml-library/libraries/fastai_example.py>`_
   * - `Gensim <https://radimrehurek.com/gensim/>`_
     - model
     - `Word2vec Example <https://github.com/operatorai/modelstore/blob/main/examples/examples-by-ml-library/libraries/gensim_example.py>`_
   * - `Keras <https://keras.io/>`_
     - model, optimizer
     - `Keras Example <https://github.com/operatorai/modelstore/blob/main/examples/examples-by-ml-library/libraries/keras_example.py>`_
   * - `LightGBM <https://lightgbm.readthedocs.io>`_
     - model
     - `LightGBM Example <https://github.com/operatorai/modelstore/blob/main/examples/examples-by-ml-library/libraries/lightgbm_example.py>`_
   * - `Mxnet <https://mxnet.apache.org>`_
     - model, epoch
     - `Mxnet Example <https://github.com/operatorai/modelstore/blob/main/examples/examples-by-ml-library/libraries/mxnet_example.py>`_
   * - `Onnx <https://onnx.ai/>`_
     - model
     - `Onnx Example <https://github.com/operatorai/modelstore/blob/main/examples/examples-by-ml-library/libraries/onnx_example.py>`_
   * - `PyTorch <https://pytorch.org/>`_
     - model, optimizer
     - `PyTorch Example <https://github.com/operatorai/modelstore/blob/main/examples/examples-by-ml-library/libraries/pytorch_example.py>`_
   * - `PyTorch Lightning <https://www.pytorchlightning.ai/>`_
     - model, trainer
     - `PyTorch Lightning Example <https://github.com/operatorai/modelstore/blob/main/examples/examples-by-ml-library/libraries/pytorch_lightning_example.py>`_
   * - `scikit-learn <https://scikit-learn.org>`_
     - model
     - `scikit-learn Example <https://github.com/operatorai/modelstore/blob/main/examples/examples-by-ml-library/libraries/sklearn_example.py>`_
   * - `skorch <https://skorch.readthedocs.io/en/stable/>`_
     - model
     - `skorch Example <https://github.com/operatorai/modelstore/blob/main/examples/examples-by-ml-library/libraries/skorch_example.py>`_
   * - `shap <https://shap.readthedocs.io/en/latest/>`_
     - explainer
     - `shap Example <https://github.com/operatorai/modelstore/blob/main/examples/examples-by-ml-library/libraries/shap_example.py>`_
   * - `Tensorflow <https://www.tensorflow.org/>`_
     - model
     - `Tensorflow Example <https://github.com/operatorai/modelstore/blob/main/examples/examples-by-ml-library/libraries/tensorflow_example.py>`_
   * - `Transformers <https://github.com/huggingface/transformers>`_
     - config, model, tokenizer
     - `Transformers Example <https://github.com/operatorai/modelstore/blob/main/examples/examples-by-ml-library/libraries/transformers_example.py>`_
   * - `XGBoost <https://xgboost.readthedocs.io>`_
     - model
     - `XGBoost Example <https://github.com/operatorai/modelstore/blob/main/examples/examples-by-ml-library/libraries/xgboost_example.py>`_


What to do if a library is not supported
----------------------------------------

If you are using a machine learning library that is not listed above, you can still use model store to upload and version your models. You will not be able to use :code:`load()` but you will be able to :code:`download()` them back.

.. code-block:: python

    model_path = save_model()

    model_store.upload("my-domain", model=model_path)

You can also:

- Let us know by `raising an issue <https://github.com/operatorai/modelstore/issues>`_
- Add support for the library by following `this guide <https://github.com/operatorai/modelstore/blob/main/modelstore/models/CONTRIBUTING.md>`_.


Uploading more than one model file
----------------------------------

There are some cases where you may want to upload two models together. 

This library supports uploading multiple models, as long as their keyword arguments do not overlap. 

For example, you might want to upload a classifier **and** a shap explainer together:

.. code-block:: python

    clf = RandomForestClassifier()
    clf.fit(X_train, y_train)

    explainer = shap.TreeExplainer(model)

    model_store.upload("my-domain", model=model, explainer=explainer)

When you load these models, model store returns a dictionary with both models:

.. code-block:: python

    models = modelstore.load(model_domain, model_id)
    clf = models["sklearn"]
    explainer = models["shap"]
