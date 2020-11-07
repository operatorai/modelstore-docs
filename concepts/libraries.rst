Supported Machine Learning Libraries
=======================================

This library currently supports:

* `CatBoost <https://catboost.ai/>`_
* `Keras <https://keras.io/>`_
* `PyTorch <https://pytorch.org/>`_
* `Scikit-Learn <https://scikit-learn.org>`_
* `Tensorflow <https://www.tensorflow.org/>`_
* `Transformers <https://github.com/huggingface/transformers>`_
* `XGBoost <https://xgboost.readthedocs.io>`_

The common pattern, across all supported libraries, is to::


   # Create an instance of the model store
   from modelstore import ModelStore

   model_store = ModelStore.from_gcloud(
      project_name="my-project",
      bucket_name="my-bucket",
   )

   # Upload your model by calling `upload()`
   meta_data = model_store.<library-name>.upload("my-domain", ...)

CatBoost
------------

To export a `CatBoost <https://catboost.ai/>`_ model, use::

    # Train your model
    model = ctb.CatBoostClassifier(loss_function="MultiClass")
    model.fit(x, y)

    # Upload the model
    meta_data = model_store.catboost.upload("my-domain", model=clf, pool=df)

This will store a multiple formats of your model to the model store:

* CatBoost binary format
* JSON
* Onnx 

The :code:`pool` argument is required `if you are training a multi class model <https://catboost.ai/docs/concepts/python-reference_catboost_save_model.html>`_.

The stored model will also contain a :code:`model_attributes.json` file with all of the attributes of the model.

Keras
-------

To export a `Keras <https://keras.io/>`_ model, use::

    # Train your model
    model = keras.Model(inputs, outputs)
    model.compile(optimizer="adam", loss="mean_squared_error")
    model.fit(X_train, y_train, epochs=10)
    # ...

    # Upload the model
    meta_data = model_store.keras.upload("my-domain", model=net, optimizer=optim)

This will add two dumps of the model into the archive; based on calling :code:`model.to_json()` and :code:`model.save()`. 

PyTorch
-------

To export a `PyTorch <https://pytorch.org/>`_ model, use::

    # Train your model
    net = ExampleNet()
    optim = ExampleOptim()
    # ...

    # Upload the model
    meta_data = model_store.pytorch.upload("my-domain", model=net, optimizer=optim)

This will add two dumps of the model into the archive; a :code:`checkpoint.pt` that
contains the net and optimizer's state (e.g., to continue training at a later date),
and a :code:`model.pt` that is the result of :code:`torch.save` with the model only
(e.g., for inference). 

Scikit-Learn
------------

To export a `scikit-learn <https://scikit-learn.org>`_ model, use::

    # Train your model
    clf = RandomForestClassifier(n_estimators=10)
    clf = clf.fit(X, Y)

    # Upload the model
    meta_data = model_store.sklearn.upload("my-domain", model=clf)

This will add a :code:`joblib` dump of the model into the archive.

Tensorflow
------------

To export a `tensorflow <https://www.tensorflow.org/>`_ model, use::

    # Train your model
    model = tf.keras.models.Sequential(
        [
            tf.keras.layers.Dense(5, activation="relu", input_shape=(10,)),
            tf.keras.layers.Dropout(0.2),
            tf.keras.layers.Dense(1),
        ]
    )
    model.compile(optimizer="adam", loss="mean_squared_error")
    model.fit(X_train, y_train, epochs=10)

    # Upload the model
    meta_data = model_store.tensorflow.upload("my-domain", model=model)

This will both save the weights (as a checkpoint file) and export/save the entire model.

Transformers
------------

To export a `transformers <https://github.com/huggingface/transformers>`_ model, use::

    # Get a pre-trained model and fine tune it
    model_name = "distilbert-base-cased"
    config = AutoConfig.from_pretrained(
        model_name, num_labels=2, finetuning_task="mnli",
    )
    tokenizer = AutoTokenizer.from_pretrained(model_name)
    model = AutoModelForSequenceClassification.from_pretrained(
        model_name, config=config,
    )

    # Upload the model
    meta_data = model_store.transformers.upload(
        "my-domain", config=config, model=model, tokenizer=tokenizer,
    )

The :code:`config` and :code:`tokenizer` parameters are optional. This will use the :code:`save_pretrained()` function to save your model.

XGBoost
-------

To export an `XGBoost <https://xgboost.readthedocs.io>`_ model, use::

    # Train your model
    bst = xgb.train(param, dtrain, num_round)

    # Upload the model
    meta_data = model_store.xgboost.upload("my-domain", model=bst)

This will add two dumps of the model into the archive; a model dump (in
an interchangeable format, for loading again later), and a model save (in JSON format, which, to date, is experimental).

