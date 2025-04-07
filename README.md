# IOHClustering

**IOHClustering** provides an interface for clustering problems, allowing users to map a dataset and a specified number of clusters to the IOHprofiler problem format. This enables seamless integration with the IOHprofiler framework for further analysis and performance evaluation.

As part of the IOHprofiler framework, IOHClustering is under active development. Some features may still be evolving, with potential updates to functionality and interfaces. The package aims to complement the [IOHanalyzer web-version](https://iohanalyzer.liacs.nl/) by offering advanced clustering capabilities and enabling large-scale data processing, which is not supported in the web version.

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

### Example: Clustering with Random Search

Below is an example of how to use **IOHClustering** to create a clustering problem and solve it using a simple random search algorithm.

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
clustering_problem = create_cluster_problem(dataset, k=2)

# Define a simple random search algorithm
class RandomSearch:
    """Simple random search algorithm"""
    def __init__(self, budget_factor: int):
        self.budget_factor: int = budget_factor
        
    def __call__(self, problem: ioh.problem.RealSingleObjective) -> None:
        """Evaluate the problem `budget_factor * DIM` times with a randomly generated solution"""
        for _ in range(self.budget_factor * problem.meta_data.n_variables):
            # Generate a random solution within the problem bounds
            x = np.random.uniform(problem.bounds.lb, problem.bounds.ub)            
            problem(x) 

# Set up a logger to store results
logger = ioh.logger.Analyzer(
    root=os.getcwd(),                  # Store data in the current working directory
    folder_name="RS_Test",             # in a folder named: 'RS_Test'
    algorithm_name="RandomSearch",    # meta-data for the algorithm used to generate these results
)

# Attach the logger and run the random search
RS = RandomSearch(budget_factor=2000)      
clustering_problem.attach_logger(logger)      
RS(clustering_problem)

# Close the logger after the run
logger.close()
```

This example demonstrates how to define a clustering problem, use a simple random search algorithm to solve it, and log the results for further analysis.

### Example: Custom Function for Clustering Problem

You can define a custom function to evaluate clustering solutions. Below is an example of how to create and use a custom evaluation function:

```python
from iohclustering import create_cluster_problem, general_cluster_metric
import numpy as np

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
    evaluation_function=clustering_function
)
```

This example demonstrates how to define a custom evaluation function for clustering problems and integrate it into the **IOHClustering** framework.

## Tutorials

Coming soon!

## License



## Acknowledgments



## Cite Us

Citation information coming soon!
