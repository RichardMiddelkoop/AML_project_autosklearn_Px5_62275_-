Reminder to Windows and Mac OSX user:

    auto-sklearn is not or partially supported on these systems, consider using Colab, VM, Docker.
    https://automl.github.io/auto-sklearn/master/installation.html#windows-osx-compatibility

General workflow for colab user
This notebook is organized in multiple steps, grouped in sections and subsections.

    Set up
        install auto-sklearn
        mount google drive
        import packages
        set paths for save/load
    STEP A - Fetch datasets from OpenML (run once)
        using sklearn API to fetch (multiple) datasets from OpenML
        save each dataset as a pickle (.pkl), containing a dict with 'x', 'y' as keys and data as values, named as openml_datasetID.pkl
        this step is intended to be run once only, afterwards user can load the data from pickle rather than fetch again which is time consuming. If you already have datasets in data folder, skip STEP A.
    STEP B - Compute meta features (optional)
        compute the meta features from pickle files, and write result to 'meta_features.pkl'
        as meta info
        to test whether the data loading works correctly
        to test whether the saving result works correctly
        can skip to STEP C
    STEP C - Run experiment
        Set up
            set up experiment by choosing budgets, datasets etc.
            set up autosklearn classifier or modify settings
            generates seed for reproducbility
            check estimated time to run experiment, make sure you won't encounter session timeout
        Run
            proceed to start experiment, a nested loop over datasets (outer) and seeds (inner) repeating the procedure
                train-test-split
                train classifier on train set
                    evaluate balanced accuracy on test set
            generate an unique timestamp id at the beginning
            results are gathered in a 'res' dict with keys 'cls' and 'acc' containing the trained classifier and test accuracy
        Save (recommended)
            save the 'res' dict to 'experiment_id.pkl' in results folder
            storage warning: file size ~ 1.2GB for 190 trained classifiers + acc, make sure you have sufficient space, else do not save classifiers
            avoid results loss in colab due to session timeout
            analyze results in separate notebook to keep running experiment
            (to do) saving results after each iteration of outer loop

Particular instruction to colab user:

    'Run for colab' contains two cells, each with function to
        install auto-sklearn on colab, then auto restart runtime. Run only once.
        mount google drive for loading/saving datasets and results. In the examples in this notebook, dataset can be load from pickle and results can be dump to pickle, both stored at user-defined directories in google drive.

Particular instruction to non-colab user:

    Ignore 'Run for colab'
    Change the paths in 'Set paths' section
    
Structure of files:

    Three different types of experiments are divided in the three directories.
    The experiments related to grid search are located in grid_search
    The experiments related to random search are located in random_search
    The experiments related to seed selection are located in seed_selection
