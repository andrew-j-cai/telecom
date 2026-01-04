# Telecommunications Network Optimizer

## Overview

A solver for an NP-hard network optimization problem: given a map of cities and the distances between them, decide which cities should have cell towers and how to connect them with fiber cables to minimize communication costs.

**Constraints:**
- Every city must either have a cell tower or be adjacent to a city with one
- The fiber network must form a tree (no redundant cables)
- Goal: minimize the average communication distance between all towers

Full technical spec is provided as a PDF.

This implementation ranked in the top 20 (based on how well the solver outputs approximate the true solutions) out of all submissions in CS 170 @ UC Berkeley (Spring 2020), completed as a solo project.

## Algorithm

**Formal Problem:** Given a positive weighted, connected, undirected graph G = (V,E), find a subgraph T of G such that:
- Every vertex v ∈ V is either in T or adjacent to a vertex in T
- T is a tree (dominating tree)
- The average pairwise distance between all vertices in T is minimized

The solver uses a greedy approach that:
1. Generates candidate spanning trees using multiple strategies (Dijkstra, BFS, MST)
2. Iteratively prunes edges from leaves to create dominating trees
3. Validates that all vertices remain dominated
4. Selects the tree with minimum average pairwise distance

The algorithm is purely deterministic.

## Installation

```bash
pip install -r requirements.txt
```

Or install dependencies directly:
```bash
pip install networkx
```

## Usage

**Solve a single input:**
```bash
python3 solver.py examples/small-1.in
```

This will output the average pairwise distance and write the solution to `test.out`.

**Solve all inputs in a directory:**

To batch process multiple inputs, create an `inputs/` directory with your `.in` files, then run:
```bash
python3 max_st.py
```

This will process all `.in` files in the `inputs/` directory and write solutions to the `outputs/` directory.

## Project Structure

- `solver.py`: Core algorithm implementation
- `max_st.py`: Batch processing script for multiple inputs
- `parse.py`: Functions to read/write graph inputs and outputs
- `utils.py`: Graph validation and cost calculation functions
- `examples/`: Sample input graphs for each size category:
  - `small-1.in`: 25 vertices
  - `medium-1.in`: 50 vertices
  - `large-1.in`: 100 vertices
- `outputs/`: Generated solution files (created on first run)

## Input/Output Format

**Input:** Each `.in` file specifies a weighted undirected graph:
- First line: number of vertices |V|
- Following lines: space-separated "u v weight" defining edges
  - u, v are vertex indices (0 to |V|-1)
  - weight is a positive float (0 < weight < 100, max 3 decimal places)

**Output:** Each `.out` file specifies a dominating tree:
- First line: space-separated list of vertices in the tree

- Following lines: space-separated "u v" pairs defining tree edges

