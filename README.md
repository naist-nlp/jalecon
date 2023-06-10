# jalecon




## Cross-Validation Splits

The file `cv_splits.json` contains cross-validation splits that we have used for experiment in [TODO add reference], and which can therefore serve to replicate it, or easily compare our results with other systems.

We have used:
- 5 folds,
- stratification by genre (News and Government),
- grouping by sequence of sentences (see Section 3 in the paper).

Genres are thus equally represented in each fold, and a sequence of sentences is never split between a training set and a validation set.

The `cv_splits.json` file has the following form:

```
[
	[split1_train, split1_validation],
	[split2_train, split2_validation],
	[split3_train, split3_validation],
	[split4_train, split4_validation],
	[split5_train, split4_validation]
]
```

Each split's train or validation set is a list of `sent_id`s, which correspond to the first fields of sentence lines in the dataset files. [TODO edit so that it corresponds to our dataset description]