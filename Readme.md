LIPO is a package for derivative-free, global optimization. Is based on
the `dlib` package and provides wrappers around its optimization routine.

The algorithm outperforms random search - sometimes by margins as large as 10000x. It is often preferable to 
Bayesian optimization which requires "tuning of the tuner". Performance is on par with moderately to well tuned Bayesian 
optimization.

The provided implementation has the option to automatically enlarge the search space if bounds are found to be 
too restrictive (i.e. the optimum being to close to one of them).

See the [LIPO algorithm implementation](http://dlib.net/python/index.html#dlib.find_max_global) for details.

A [great blog post](http://blog.dlib.net/2017/12/a-global-optimization-algorithm-worth.html) by the author of 
`dlib` exists, describing how it works.

# Installation

Execute

`pip install lipo`

# Usage

```python
from lipo import GlobalOptimizer

def function(x, y, z):
    zdict = {"a": 1, "b": 2}
    return -((x - 1.23) ** 6) + -((y - 0.3) ** 4) * zdict[z]

pre_eval_x = dict(x=2.3, y=13, z="b")
evaluations = [(pre_eval_x, function(**pre_eval_x))]

search = GlobalOptimizer(
    function,
    lower_bounds={"x": -10.0, "y": -10},
    upper_bounds={"x": 10.0, "y": -3},
    categories={"z": ["a", "b"]},
    evaluations=evaluations,
    maximize=True,
)

num_function_calls = 1000
search.run(num_function_calls)
```

The optimizer will automatically extend the search bounds if necessary.

Further, the package provides an implementation of the scikit-learn interface for 
hyperparamter search.

```python
from lipo import LIPOSearchCV

search = LIPOSearchCV(
    estimator,
    param_space={"param_1": [0.1, 100], "param_2": ["category_1", "category_2"]},
    n_iter=100
)
search.fit(X, y)
print(search.best_params_)
```