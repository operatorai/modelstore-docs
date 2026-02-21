Additional upload functionality
===============================

Uploading more than one model file
----------------------------------

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

Uploading extra files with the model
------------------------------------

This library supports uploading a model with one or more extra files.

For example, you might want to upload a classifier **and** the predictions it made on the test set.

.. code-block:: python

    clf = RandomForestClassifier()
    clf.fit(X_train, y_train)

    predictions = clf.predict(X_test)
    file_path = "predictions.csv"
    numpy.savetxt(file_path, predictions, delimiter=",")

    modelstore.upload("my-domain", model=model, extra_files=file_path)

When you load these models, the extra files are not loaded into memory:

.. code-block:: python

    clf = modelstore.load(model_domain, model_id)

Adding extra metadata to a model
---------------------------------

You can attach arbitrary key-value metadata to any upload using the :code:`extra_metadata` keyword argument:

.. code-block:: python

    meta_data = model_store.upload(
        "my-domain",
        model=clf,
        extra_metadata={"accuracy": 0.95, "dataset": "v2"},
    )

Using a custom model ID
-----------------------

By default, modelstore assigns a UUID4 to each uploaded model. You can override this by passing a :code:`model_id` argument:

.. code-block:: python

    meta_data = model_store.upload(
        "my-domain",
        model=clf,
        model_id="my-custom-id-v1",
    )

Model IDs may contain alphanumeric characters, hyphens, underscores, and periods. If the model ID already exists in the domain, a :code:`ModelExistsException` is raised.
