### Hungarian Algorithm Project Enhancement

#### Project Overview
This project implements the Hungarian Algorithm, a classic optimization algorithm used to solve assignment problems. The primary objective is to find the most cost-effective assignment in a weighted bipartite graph, typically representing workers and jobs. The algorithm aims to minimize the total cost of assigning each worker to a specific job, considering that each worker-job assignment has a specific cost associated with it.

#### Real-World Problem Definition
The assignment problem is a fundamental question in operations research and computer science, especially in scenarios like workforce allocation, resource management, or task scheduling. The goal is to find the most efficient way to assign a set of resources (like workers) to a set of tasks (like jobs), where each assignment carries a certain cost or efficiency metric. The Hungarian Algorithm is a powerful tool for solving such problems, especially when the number of resources and tasks is the same, or can be made the same through padding.

#### Graph Representation
In this context, the graph is a bipartite graph where one set of vertices represents workers and the other set represents jobs. Each edge in the graph has a weight, which represents the cost or efficiency of assigning the corresponding worker to the job. The solution of the Hungarian Algorithm can be visualized as a set of edges where each worker vertex is connected to exactly one job vertex, and the total weight (cost) of these edges is minimized.
![image](https://github.com/gokturkhatay/Hungarian-Algorithm/assets/36425290/88dc42a9-6ba8-4f71-8c8c-7ba7778daf09)  
#### Code Explanation and Completion
The Python code for implementing the Hungarian Algorithm is enhanced for better understanding and functionality:

```python
import numpy as np
from scipy.optimize import linear_sum_assignment

def read_cost_matrix():
    """
    Reads the cost matrix from user input.

    Returns:
    A 2D numpy array representing the cost matrix.
    """
    rows = int(input("Enter the number of rows (workers): "))
    cost_matrix = []

    for i in range(rows):
        row_input = input(f"Enter the costs for row {i + 1} separated by spaces: ")
        row = [int(x) for x in row_input.split()]
        cost_matrix.append(row)

    return np.array(cost_matrix)

def hungarian_algorithm(cost_matrix):
    """
    Apply the Hungarian Algorithm to find the minimum cost assignment.

    Args:
    cost_matrix: A 2D numpy array where element (i, j) represents the cost of assigning worker i to job j.

    Returns:
    A list of tuples, each representing an assignment (worker, job).
    """
    n = max(cost_matrix.shape)
    max_cost = np.max(cost_matrix)
    padded_matrix = np.pad(cost_matrix, ((0, n - cost_matrix.shape[0]), (0, n - cost_matrix.shape[1])),
                           mode='constant', constant_values=(0, max_cost))

    for i in range(n):
        row_min = np.min(padded_matrix[i])
        padded_matrix[i] -= row_min

    for j in range(n):
        col_min = np.min(padded_matrix[:, j])
        padded_matrix[:, j] -= col_min

    row_ind, col_ind = linear_sum_assignment(padded_matrix)

    return [(i, j) for i, j in zip(row_ind, col_ind) if i < cost_matrix.shape[0] and j < cost_matrix.shape[1]]

# Main program
print("Hungarian Algorithm for Assignment Problem")
cost_matrix = read_cost_matrix()
assignments = hungarian_algorithm(cost_matrix)
print("Optimal Assignments:", assignments)
```

### Code Explanation with User Input

1. **User Input for Cost Matrix**:
   - The program starts with a function `read_cost_matrix` which prompts the user to input the size of the cost matrix (number of workers) and then the costs for each row.
   - Each row of the cost matrix represents the costs of assigning a particular worker to various jobs. The user enters these costs as space-separated values.

2. **Hungarian Algorithm Function (`hungarian_algorithm`)**:
   - The `hungarian_algorithm` function remains the core of the implementation. It takes the user-provided cost matrix as input.
   - The function first ensures that the cost matrix is square. If not, it pads the matrix with zeros (or maximum cost value if needed) to make it square. This step is essential for the algorithm to work correctly in scenarios where the number of workers and jobs is not equal.
   - The algorithm then proceeds with row and column reductions. In row reduction, the minimum value of each row is subtracted from all elements in that row. Column reduction follows a similar approach.
   - After the reductions, the `linear_sum_assignment` function from SciPy is used. This function performs complex steps of the Hungarian Algorithm, including covering zeros with a minimum number of lines and finding the optimal assignment.

3. **Finding Optimal Assignments**:
   - The function returns a list of tuples representing the optimal assignments. Each tuple contains two elements: the index of the worker and the index of the job they are assigned to.
   - The result excludes any assignments related to the padded rows and columns, thus reflecting the actual solution for the original problem.

4. **Main Program**:
   - The main part of the program prompts the user for input and then processes this input through the Hungarian Algorithm to find the optimal assignments.
   - After processing, it prints out the optimal assignments, showing which worker is assigned to which job for the minimum total cost.# Hungarian-Algorithm


     ![image](https://github.com/gokturkhatay/Hungarian-Algorithm/assets/36425290/b991afbe-ea13-44f6-8bef-6e4d512d3d52)

