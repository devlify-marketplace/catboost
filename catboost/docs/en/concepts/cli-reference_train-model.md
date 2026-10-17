# Train a model

{% note info %}

Training on GPU requires NVIDIA Driver of version 450.80.02 or higher.

{% endnote %}

## Execution format {#execution-format}

```text
catboost fit -f <file path> [optional parameters]
```

## Options {#options}

See all options in the [Training parameters](../references/training-parameters/index.md) section.

## Usage examples {#usage-examples}

Train a model with 100 trees on a comma-separated pool with header:

```text
catboost fit --learn-set train.csv --test-set test.csv --column-description train.cd  --loss-function RMSE --iterations 100 --delimiter=',' --has-header
```

Train a classification model on GPU:

```text
catboost fit --learn-set ../pytest/data/adult/train_small --column-description ../pytest/data/adult/train.cd --task-type GPU
```

## Custom objectives and metrics {#custom-objectives-and-metrics}

Custom objectives and metrics for the CLI are implemented in C++ as part of the CatBoost source. The CLI does not load a custom implementation from a Python file or class, and it does not support external plugins. After modifying the source, rebuild the CLI binary before selecting the implementation. See the [custom metric tutorial](https://github.com/catboost/catboost/blob/master/catboost/tutorials/custom_loss/custom_metric_tutorial.md) for implementation details.

- **Custom metric:** implement `TUserDefinedPerObjectMetric` for a per-object metric or `TUserDefinedQuerywiseMetric` for a querywise metric. Select it with `--eval-metric UserPerObjMetric` or `--eval-metric UserQuerywiseMetric`, respectively.
- **Custom objective:** implement `TUserDefinedPerObjectError` for a per-object objective or `TUserDefinedQuerywiseError` for a querywise objective. Select it with `--loss-function UserPerObjMetric` or `--loss-function UserQuerywiseMetric`, respectively.

`UserPerObjMetric` and `UserQuerywiseMetric` are names for implementations compiled into the CLI; they are not paths to source files or Python classes. Python custom objectives and metrics must be used through the Python API, not passed to the CLI.
