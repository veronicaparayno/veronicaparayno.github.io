---
layout: project
type: project
image: img/ALGOPIC.jpeg
title: "Dijkstra's algorithm"
date: 2023
published: true
labels:
  - Project
  - Algorithm
  - Coding
summary: "Implement Dijkstra's Algorithm"
---


## Project Overview: Dijkstra's Algorithm Implementation

## Introduction:
In this project, you will delve into the world of graph theory by implementing Dijkstra's algorithm. The primary goal is to create a class, `Weighted_graph`, which allows users to construct, manipulate, and explore undirected weighted graphs. This class facilitates the calculation of the shortest distance between connected vertices, contributing to a fundamental understanding of graph-based algorithms.

# Project Specifications:
1. Class: Weighted_graph
   - Constructors:
     - `Weighted_graph(int n = 50)`: Construct an undirected graph with n vertices (default is 50). If n ≤ 0, use n = 1.
   - Destructor:
     - `~Weighted_graph()`: Clean up any allocated memory.
   - Accessors:
     - `int degree(int n) const`: Returns the degree of the vertex n. Throws an illegal argument exception if the argument does not correspond to an existing vertex. (O(1))
     - `int edge_count() const`: Returns the number of edges in the graph. (O(1))
     - `double adjacent(int m, int n) const`: Returns the weight of the edge connecting vertices m and n. If the vertices are the same, return 0. If the vertices are not adjacent, return infinity. Throws an illegal argument exception if the arguments do not correspond to existing vertices.
   - Mutators:
     - `void insert(int m, int n, double w)`: If the weight w ≤ 0 or is infinity, throws an illegal argument exception. If w > 0, adds an edge between vertices m and n. If an edge already exists, replaces the weight of the edge with the new weight. Throws an illegal argument exception if the vertices do not exist or are equal.
     - `double distance(int m, int n)`: Returns the shortest distance between vertices m and n. Throws an illegal argument exception if the arguments do not correspond to existing vertices. The distance between a vertex and itself is 0.0. The distance between vertices that are not connected is infinity.

# Purpose 
The purpose of this project is to reinforce my understanding of the material and initiate the development of your personal library of tools. Subsequent projects may build upon the concepts and implementations from this project, emphasizing the interconnected nature of various data structures.

```/*****************************************
 * By submitting this file, I affirm that
 * I am the author of all modifications to
 * the provided code.
 *
 * The following is a list of those students
 * I had worked together in preparing this project:
 * - Veronica Parayno
 *****************************************/
#ifndef WEIGHTED_GRAPH_H
#define WEIGHTED_GRAPH_H

#ifndef nullptr
#define nullptr 0
#endif

#include <iostream>
#include <limits>
#include "Exception.h"

class Weighted_graph {
private:
    int *deg;
    int edges;
    int size;
    double **weight_adj_matrix;
    static const double INF;

public:
    Weighted_graph(int = 50);
    ~Weighted_graph();
    int degree(int) const;
    int edge_count() const;
    double adjacent(int, int) const;
    double distance(int, int) const;
    void insert(int, int, double);

    // Friends
    friend std::ostream &operator<<(std::ostream &, Weighted_graph const &);
};

const double Weighted_graph::INF = std::numeric_limits<double>::infinity();

Weighted_graph::Weighted_graph(int n) {
    if (n <= 0) {
        n = 1;
    }

    deg = new int[n];
    for (int i = 0; i < n; i++) {
        deg[i] = 0;
    }

    edges = 0;
    size = n;

    weight_adj_matrix = new double *[n];
    for (int i = 0; i < size; i++) {
        weight_adj_matrix[i] = new double[n];
    }

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            weight_adj_matrix[i][j] = INF;
        }
    }
}

Weighted_graph::~Weighted_graph() {
    delete[] deg;

    for (int i = 0; i < size; i++) {
        delete[] weight_adj_matrix[i];
    }
    delete[] weight_adj_matrix;
}

int Weighted_graph::degree(int n) const {
    if (n < 0 || n > size - 1) {
        throw illegal_argument();
    }
    return deg[n];
}

int Weighted_graph::edge_count() const {
    return edges;
}

double Weighted_graph::adjacent(int m, int n) const {
    if (n < 0 || m < 0 || n > size - 1 || m > size - 1) {
        throw illegal_argument();
    }
    if (m == n) {
        return 0;
    }
    return weight_adj_matrix[m][n];
}

double Weighted_graph::distance(int m, int n) const {
    if (n < 0 || m < 0 || n > size - 1 || m > size - 1) {
        throw illegal_argument();
    }
    bool *visited = new bool[size];
    double *dist = new double[size];

    for (int i = 0; i < size; i++) {
        visited[i] = false;
        if (m == i)
            dist[i] = 0;
        else
            dist[i] = INF;
    }

    visited[m] = true;
    int curr = m;
    int visitedCount = 1;
    int count = 0;

    for (int i = 0; i < size; i++) {
        if (deg[i] != 0)
            count++;
    }

    while (visitedCount != count) {
        for (int i = 0; i < size; i++) {
            if ((weight_adj_matrix[curr][i] != INF) && (!visited[i]) && (dist[i] > (dist[curr] + weight_adj_matrix[i][curr]))) {
                dist[i] = dist[curr] + weight_adj_matrix[i][curr];
            }
        }

        double minDistance = INF;
        int minNode = -1;

        for (int i = 0; i < size; i++) {
            if (dist[i] < minDistance && visited[i] == false) {
                minDistance = dist[i];
                minNode = i;
            }
        }

        curr = minNode;

        if (curr == -1)
            break;
        visited[curr] = true;
        visitedCount += 1;
    }

    delete[] visited;
    double distance = dist[n];
    delete[] dist;
    return distance;
}

void Weighted_graph::insert(int m, int n, double w) {
    if (w <= 0 || w == INF || n < 0 || m < 0 || n > size - 1 || m == size - 1 || m == n) {
        throw illegal_argument();
    }

    if (weight_adj_matrix[m][n] != INF) {
        weight_adj_matrix[m][n] = w;
        weight_adj_matrix[n][m] = w;
    } else {
        weight_adj_matrix[m][n] = w;
        weight_adj_matrix[n][m] = w;
        edges++;
        deg[m]++;
        deg[n]++;
    }
}

std::ostream &operator<<(std::ostream &out, Weighted_graph const &graph) {
    return out;
}

#endif
 ```



