# `GridSearchCV`
* [an example](https://scikit-learn.org/stable/auto_examples/model_selection/plot_grid_search_digits.html)
```
def svm_grid_search(train_data, train_label, eval_data, eval_label):
    tuned_parameters = [{'kernel': ['rbf'], 'gamma': [1e-3, 1e-4],
                         'C': [1, 10, 100, 1000]},
                        {'kernel': ['linear'], 'C': [1, 10, 100, 1000]}]

    clf = GridSearchCV(SVC(),
                       tuned_parameters,
                       cv=5,
                       n_jobs=-1,
                       refit=True,
                       return_train_score=True)

    logging.info("Fitting and grid search on SVM.")
    clf.fit(train_data, train_label)

    logging.info("Best parameters set found on training set:")
    logging.info(clf.best_params_)
    logging.info("Grid scores on training set:")
    means = clf.cv_results_['mean_test_score']
    stds = clf.cv_results_['std_test_score']
    for mean, std, params in zip(means, stds, clf.cv_results_['params']):
        logging.info("%0.3f (+/-%0.03f) for %r" % (mean, std * 2, params))

    logging.info("Detailed classification report:")
    logging.info("The model is trained on the full training set.")
    logging.info("The scores are computed on the full evaluation set.")
    eval_pred = clf.predict(eval_data)
    logging.info(classification_report(eval_label, eval_pred))
```
