---
layout: page
title: My Machine Learning Notes
permalink: /my_ml/
---

Nice to know (remember) python libraries and their functions, used to build & train succesfull machine learning models. Some of the code examples are taken from [Kaggle](https://www.kaggle.com).

Covered libraries are: Numpy, Pandas, Scikit learn, Tensorflow.  
{% highlight python %}
    import pandas as pd # load Pandas library

{% endhighlight %}

# Read data
{% highlight python %}
    # Read the data
    X_full = pd.read_csv('../input/train.csv', index_col='Id')
    X_test_full = pd.read_csv('../input/test.csv', index_col='Id')
{% endhighlight %}

# Load data (into subsets)
{% highlight python %}
    # Remove rows with missing target, separate target from predictors
    X_full.dropna(axis=0, subset=['SalePrice'], inplace=True)
    y = X_full.SalePrice
    X_full.drop(['SalePrice'], axis=1, inplace=True)
{% endhighlight %}

{% highlight python %}
    # Select categorical columns with relatively low cardinality (convenient but arbitrary)
    categorical_cols = [cname for cname in X_train_full.columns if X_train_full[cname].nunique() < 10 and X_train_full[cname].dtype == "object"]

    # Select numerical columns
    numerical_cols = [cname for cname in X_train_full.columns if X_train_full[cname].dtype in ['int64', 'float64']]

    # Keep selected columns only
    my_cols = categorical_cols + numerical_cols
    X_train = X_train_full[my_cols].copy()
    X_valid = X_valid_full[my_cols].copy()
{% endhighlight %}

# Write data
{% highlight python %}
    # Save test predictions to file
    output = pd.DataFrame({'Id': X_test.index,
                        'SalePrice': preds_test})
    output.to_csv('submission.csv', index=False)
{% endhighlight %}

# Pandas DataFrame operations
{% highlight python %}
    # Use only numerical predictors
    X = X_full.select_dtypes(exclude=['object'])
    X_test = X_test_full.select_dtypes(exclude=['object'])

    # Inspect columns having missing values
    missing_val_count_by_column = (X_train.isnull().sum())
    print(missing_val_count_by_column[missing_val_count_by_column > 0])
{% endhighlight %}

{% highlight python %}
    # Get names of columns with missing values
    X_train_missing_columns = [col for col in X_train.columns
                           if X_train[col].isnull().sum() > 0] # Your code here

    # Drop columns in training and validation data
    reduced_X_train = X_train.drop(X_train_missing_columns, axis=1)
    reduced_X_valid = X_valid.drop(X_train_missing_columns, axis=1)

{% endhighlight %}

## Manipulate data
{% highlight python %}
    from sklearn.impute import SimpleImputer

    # imputation 
    my_imputer = SimpleImputer() 
    imputed_X_train = pd.DataFrame(my_imputer.fit_transform(X_train))
    imputed_X_valid = pd.DataFrame(my_imputer.transform(X_valid))

    # imputation removed column names; put them back
    imputed_X_train.columns = X_train.columns
    imputed_X_valid.columns = X_valid.columns
{% endhighlight %}

### Pipelines
{% highlight python %}
    from sklearn.compose import ColumnTransformer
    from sklearn.pipeline import Pipeline
    from sklearn.impute import SimpleImputer
    from sklearn.preprocessing import OneHotEncoder

    # Preprocessing for numerical data
    numerical_transformer = SimpleImputer(strategy='constant')

    # Preprocessing for categorical data
    categorical_transformer = Pipeline(steps=[
        ('imputer', SimpleImputer(strategy='most_frequent')),
        ('onehot', OneHotEncoder(handle_unknown='ignore'))
    ])

    # Bundle preprocessing for numerical and categorical data
    preprocessor = ColumnTransformer(
        transformers=[
            ('num', numerical_transformer, numerical_cols),
            ('cat', categorical_transformer, categorical_cols)
        ])
{% highlight python %}

{% endhighlight %}
    from sklearn.metrics import mean_absolute_error

    # Bundle preprocessing and modeling code in a pipeline
    my_pipeline = Pipeline(steps=[('preprocessor', preprocessor),
                                ('model', model)
                                ])

    # Preprocessing of training data, fit model 
    my_pipeline.fit(X_train, y_train)

    # Preprocessing of validation data, get predictions
    preds = my_pipeline.predict(X_valid)

    # Evaluate the model
    score = mean_absolute_error(y_valid, preds)
    print('MAE:', score)
{% endhighlight %}

## Evaluate data
### Cross-validation
{% highlight python %}
    from sklearn.model_selection import cross_val_score

    # Multiply by -1 since sklearn calculates *negative* MAE
    scores = -1 * cross_val_score(my_pipeline, X, y,
                                cv=5,
                                scoring='neg_mean_absolute_error')

    print("MAE scores:\n", scores)
    print("Average MAE score (across experiments):")
    print(scores.mean())
{% endhighlight %}

- Numpby operations

### Build models
{% highlight python %}
    from sklearn.model_selection import train_test_split
    # Break off validation set from training data
    X_train, X_valid, y_train, y_valid = train_test_split(X, y, train_size=0.8, test_size=0.2,
{% endhighlight %}

### Train models
{% highlight python %}
    # Define and fit model
    model = RandomForestRegressor(n_estimators=100, random_state=0)
    model.fit(final_X_train, y_train)

    # Get validation predictions and MAE
    preds_valid = model.predict(final_X_valid)
    print("MAE (Your approach):")
    print(mean_absolute_error(y_valid, preds_valid))
{% endhighlight %}

### Evaluate models
{% highlight python %}
    from sklearn.ensemble import RandomForestRegressor
    from sklearn.metrics import mean_absolute_error

    # Function for comparing different data approaches (on the same model)
    def score_dataset(X_train, X_valid, y_train, y_valid):
        model = RandomForestRegressor(n_estimators=100, random_state=0)
        model.fit(X_train, y_train)
        preds = model.predict(X_valid)
        return mean_absolute_error(y_valid, preds)
{% endhighlight %}
- Validate
