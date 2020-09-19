Scikit-Learn Example
====================

This example is based on the `GradientBoostingRegressor <https://scikit-learn.org/stable/auto_examples/ensemble/plot_gradient_boosting_regression.html#sphx-glr-auto-examples-ensemble-plot-gradient-boosting-regression-py>`_
tutorial from the scikit-learn website::

    import json
    import os

    from sklearn.datasets import load_diabetes
    from sklearn.ensemble import GradientBoostingRegressor
    from sklearn.model_selection import train_test_split

    from modelstore import ModelStore


    def train():
        diabetes = load_diabetes()
        X_train, X_test, y_train, y_test = train_test_split(
            diabetes.data, diabetes.target, test_size=0.1, random_state=13
        )
        params = {
            "n_estimators": 500,
            "max_depth": 4,
            "min_samples_split": 5,
            "learning_rate": 0.01,
            "loss": "ls",
        }
        reg = GradientBoostingRegressor(**params)
        reg.fit(X_train, y_train)
        # Skipped for brevity (but important!) evaluate the model
        return reg


    if __name__ == "__main__":
        # In this demo, we train a GradientBoostingRegressor
        # using the same approach described on the scikit-learn website.
        # Replace this with the code to train your own model
        model = train()

        # The modelstore library currently assumes you have already created
        # a Cloud Storage bucket and will raise an exception if it doesn't exist

        # This example assumes that you have the GCP project name and bucket id
        # saved as environment variables - replace the os.environ below with
        # your values
        store = ModelStore.from_gcloud(
            project_name=os.environ["GCP_PROJECT_ID"],
            bucket_name=os.environ["GCP_BUCKET_NAME"],
        )

        # Create an archive containing the trained model
        archive = store.sklearn.create_archive(model=model)

        # Optional: the archive that was created is called "artifacts.tar.gz"
        # You can add more files into this archive if you want

        # Upload the archive to the model store
        # The first string is the model's domain - which helps you to group
        # many models that are trained on the same target together
        meta = store.upload("sklearn-diabetes-boosting-demo", archive)

        # Optional: the artifacts.tar.gz file is generated into the current
        # working directory and you can remove them if you do not
        # need a local copy
        os.remove(archive)

        # The upload returns meta-data about the model that was uploaded
        # This meta-data has also been sync'ed into the cloud storage
        #  bucket
        print("✅  Finished uploading model!")
        print(json.dumps(meta, indent=4))
