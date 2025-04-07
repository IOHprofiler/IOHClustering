# IOHClustering

**IOHClustering** provides an interface for clustering problems, allowing users to map a dataset and a specified number of clusters to the IOHprofiler problem format. This enables seamless integration with the IOHprofiler framework for further analysis and performance evaluation.

As part of the IOHprofiler framework, IOHClustering is under active development. Some features may still be evolving, with potential updates to functionality and interfaces.

## Features

- Transform datasets into clustering problems compatible with the IOHprofiler framework.
- Real-world datasets for clustering, such as:
    - Iris dataset
    - Wine dataset
    - KC1 dataset
    - Glass dataset

## Installation

The minimum supported Python version is 3.10. Install IOHClustering via pip and git:

```bash
git clone https://github.com/IOHprofiler/IOHClustering.git
cd IOHClustering
pip install .
```

## Basic Usage
### Examples and Tutorials

Below are several examples demonstrating how to use **IOHClustering** for various clustering tasks. These examples cover basic usage, working with benchmark problems, solving custom datasets, and defining custom evaluation functions.

#### Example: Clustering a Dataset

The following example shows how to create a clustering problem with a known dataset using the **IOHClustering** framework:

```python
from iohclustering import create_cluster_problem

# Example dataset
dataset = "iris_pca"

# Create a clustering problem
clustering_problem, retransform = create_cluster_problem(
    dataset=dataset,
    k=2,
)

print(clustering_problem.meta_data)
```

#### Example: Listing Available Benchmark Problems

You can retrieve a list of all benchmark problems available in **IOHClustering**:

```python
from iohclustering import load_problems

problems = load_problems()

for problem in problems.keys():
    print(problem)
```

#### Example: Solving a Custom Dataset with Random Search

This example demonstrates how to solve a clustering problem using a simple random search algorithm:

```python
from iohclustering import create_cluster_problem
import ioh
import numpy as np
import os

# Example dataset
dataset = [
    [1.0, 2.0],
    [2.0, 3.0],
    [3.0, 4.0],
    [8.0, 9.0],
    [9.0, 10.0]
]

# Initialize the clustering interface
clustering_problem, retransform = create_cluster_problem(dataset, k=2)

# Define a simple random search algorithm
class RandomSearch:
    """Simple random search algorithm"""
    def __init__(self, budget_factor: int):
        self.budget_factor: int = budget_factor

    def __call__(self, problem: ioh.problem.RealSingleObjective) -> None:
        """Evaluate the problem `budget_factor * DIM` times with a randomly generated solution"""
        for _ in range(self.budget_factor * problem.meta_data.n_variables):
            x = np.random.uniform(problem.bounds.lb, problem.bounds.ub)
            problem(x)

# Set up a logger to store results
logger = ioh.logger.Analyzer(
    root=os.getcwd(),
    folder_name="RS_Test",
    algorithm_name="RandomSearch",
)

# Attach the logger and run the random search
RS = RandomSearch(budget_factor=2000)
clustering_problem.attach_logger(logger)
RS(clustering_problem)

# Close the logger after the run
logger.close()
```

This example illustrates how to define a clustering problem, solve it using random search, and log the results for further analysis.

#### Example: Custom Evaluation Function for Clustering

You can define a custom evaluation function for clustering problems. Here's an example:

```python
from iohclustering import create_cluster_problem, general_cluster_metric

# Example dataset
dataset = "iris_pca"

# Define a custom error function
def custom_error_function(X, centroids, labels):
    pass

# Define a custom distance function
def custom_distance_function(x, centroids):
    pass

clustering_function = general_cluster_metric(custom_distance_function, custom_error_function)

# Create a clustering problem with the custom function
clustering_problem = create_cluster_problem(
    dataset=dataset,
    k=2,
    error_metric=clustering_function
)
```

This example demonstrates how to integrate a custom evaluation function into the **IOHClustering** framework.



## Tutorials

Coming soon!

## License



## Acknowledgments



## Cite Us

Citation information coming soon!