Supported Machine Learning Libraries
=======================================

This library currently supports:

* :ref:`Catboost<Catboost>`
* :ref:`Fast AI<Fast AI>`
* :ref:`Gensim<Gensim>`
* :ref:`Keras<Keras>`
* :ref:`LightGBM<LightGBM>`
* :ref:`PyTorch<PyTorch>`
* :ref:`PyTorch Lightning<PyTorch Lightning>`
* :ref:`Scikit-Learn<Scikit-Learn>`
* :ref:`Tensorflow<Tensorflow>`
* :ref:`Transformers<Transformers>`
* :ref:`XGBoost<XGBoost>`

The common pattern, across all supported libraries, is to::


   # Create an instance of the model store
   from modelstore import ModelStore

   model_store = ModelStore.from_gcloud(
      project_name="my-project",
      bucket_name="my-bucket",
   )

   # Upload your model by calling `upload()`
   model_store.<library-name>.upload("my-domain", ...)

CatBoost
--------

To export a `CatBoost <https://catboost.ai/>`_ model, use::

    # Train your model
    model = ctb.CatBoostClassifier(loss_function="MultiClass")
    model.fit(x, y)

    # Upload the model
    model_store.catboost.upload("my-domain", model=clf, pool=df)

This will store a multiple formats of your model to the model store:

* CatBoost binary format
* JSON
* Onnx 

The :code:`pool` argument is required `if you are training a multi class model <https://catboost.ai/docs/concepts/python-reference_catboost_save_model.html>`_. The stored model will also contain a :code:`model_attributes.json` file with all of the attributes of the model.

Fast AI
-------

To export a `FastAI <https://github.com/fastai/fastai/>`_ model, use::

    # Train your model
    learner = tabular_learner(dl)

    learner.fit_one_cycle(n_epoch=1)

    # Upload the model
    model_store.fastai.upload("my-domain", learner=learner)

This will create two dumps of the model, based on :code:`learner.save()` and :code:`learner.export()`.

Gensim
------

To export a `Gensim <https://radimrehurek.com/gensim/>`_ model, use::

    # Train your model
    model = word2vec.Word2Vec(sentences, min_count=2)

    # Upload the model
    model_store.gensim.upload("my-domain", model=model)

This will save the model (using :code:`model.save()`) and, if present, will separately save the word vectors (using :code:`model.wv.save()`).

Keras
-----

To export a `Keras <https://keras.io/>`_ model, use::

    # Train your model
    model = keras.Model(inputs, outputs)
    model.compile(optimizer="adam", loss="mean_squared_error")
    model.fit(X_train, y_train, epochs=10)
    # ...

    # Upload the model
    model_store.keras.upload("my-domain", model=net, optimizer=optim)

This will create two dumps of the model, based on calling :code:`model.to_json()` and :code:`model.save()`. 

LightGBM
--------

To export a `LightGBM <https://lightgbm.readthedocs.io>`_ model, use::

    # Train your model
    model = lgb.train(param, train_data, num_round, valid_sets=[validation_data])
    # ...

    # Upload the model
    model_store.lightgbm.upload(model_domain, model=model)

This will create two dumps of the model, based on calling :code:`model.save_model()` and :code:`model.dump_model()`. 

PyTorch
-------

To export a `PyTorch <https://pytorch.org/>`_ model, use::

    # Train your model
    net = ExampleNet()
    optim = ExampleOptim()
    # ...

    # Upload the model
    model_store.pytorch.upload("my-domain", model=net, optimizer=optim)

This will create two dumps of the model; a :code:`checkpoint.pt` that contains the net and optimizer's state (e.g., to continue training at a later date), and a :code:`model.pt` that is the result of :code:`torch.save` with the model only (e.g., for inference). 

PyTorch Lightning
-----------------

To export a `PyTorch Lightning <https://www.pytorchlightning.ai/>`_ model, use::

    # Train your model
    model = ExampleLightningNet()
    trainer = pl.Trainer(max_epochs=5, default_root_dir=mkdtemp())
    trainer.fit(model, train_dataloader, val_dataloader)

    # Upload the model
    model_store.pytorch_lightning.upload(
        model_domain, trainer=trainer, model=model
    )

This will create a dump of the model; based on calling the :code:`trainer.save_checkpoint(file_path)` function. 

Scikit-Learn
------------

To export a `scikit-learn <https://scikit-learn.org>`_ model, use::

    # Train your model
    clf = RandomForestClassifier(n_estimators=10)
    clf = clf.fit(X, Y)

    # Upload the model
    model_store.sklearn.upload("my-domain", model=clf)

This will create a :code:`joblib` dump of the model.

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
    model_store.tensorflow.upload("my-domain", model=model)

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
    model_store.transformers.upload(
        "my-domain", config=config, model=model, tokenizer=tokenizer,
    )

The :code:`config` and :code:`tokenizer` parameters are optional. This will use the :code:`save_pretrained()` function to save your model.

XGBoost
-------

To export an `XGBoost <https://xgboost.readthedocs.io>`_ model, use::

    # Train your model
    bst = xgb.train(param, dtrain, num_round)

    # Upload the model
    model_store.xgboost.upload("my-domain", model=bst)

This will add two dumps of the model into the archive; a model dump (in
an interchangeable format, for loading again later), and a model save (in JSON format, which, to date, is experimental).

