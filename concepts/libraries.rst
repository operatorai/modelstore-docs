Supported Machine Learning Libraries
=======================================

This library currently supports:

* `CatBoost <https://catboost.ai/>`_
* `Keras <https://keras.io/>`_
* `PyTorch <https://pytorch.org/>`_
* `Scikit-Learn <https://scikit-learn.org>`_
* `XGBoost <https://xgboost.readthedocs.io>`_

The common pattern, across all supported libraries, is to::


   # Create an instance of the model store
   from modelstore import ModelStore

   ms = ModelStore.from_gcloud(
      project_name="my-project",
      bucket_name="my-bucket",
   )

   # Export your model by calling `create_archive()`
   archive = ms.<library-name>.create_archive(**kwargs)

   # Upload your model by calling `upload()`
   meta_data = ms.upload("model-domain", archive)

CatBoost
------------

To export a `CatBoost <https://catboost.ai/>`_ model, use::

    # Train your model
    model = ctb.CatBoostClassifier(loss_function="MultiClass")
    model.fit(x, y)

    # Create an archive
    archive = ms.catboost.create_archive(model=clf, pool=df)

This will add a multiple formats of your model to the archive:

* CatBoost binary format
* JSON
* Onnx 

The :code:`pool` argument is required `if you are training a multi class model <https://catboost.ai/docs/concepts/python-reference_catboost_save_model.html>`_.

The archive will also contain a :code:`model_attributes.json` file with all of the
attributes of the model.

Keras
-------

To export a `Keras <https://keras.io/>`_ model, use::

    # Train your model
    model = keras.Model(inputs, outputs)
    model.compile(optimizer="adam", loss="mean_squared_error")
    model.fit(X_train, y_train, epochs=10)
    # ...

    # Create and upload an archive
    archive = ms.keras.create_archive(model=net, optimizer=optim)
    rsp = ms.upload("model-domain", archive)

This will add two dumps of the model into the archive; based on calling :code:`model.to_json()` and :code:`model.save()`. 

PyTorch
-------

To export a `PyTorch <https://pytorch.org/>`_ model, use::

    # Train your model
    net = ExampleNet()
    optim = ExampleOptim()
    # ...

    # Create and upload an archive
    archive = ms.pytorch.create_archive(model=net, optimizer=optim)
    rsp = ms.upload("model-domain", archive)

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

    # Create an archive
    archive = ms.sklearn.create_archive(model=clf)

This will add a :code:`joblib` dump of the model into the archive.

XGBoost
-------

To export an `XGBoost <https://xgboost.readthedocs.io>`_ model, use::

    # Train your model
    bst = xgb.train(param, dtrain, num_round)

    # Create and upload an archive
    archive = ms.xgboost.create_archive(model=bst)
    rsp = ms.upload("model-domain", archive)

This will add two dumps of the model into the archive; a model dump (in
an interchangeable format, for loading again later), and a model save (in JSON format, which, to date, is experimental).

