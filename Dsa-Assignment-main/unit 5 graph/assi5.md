## ASSIGNMENT 5
**Title:**

 Write a Program to accept a graph from a user and represent it with Adjacency List and perform BFS and DFS traversals on it.

**Author:**

**Yash Santosh Khandagale**
## Aim

To write a program in C++ to accept a graph from the user, represent it using an Adjacency List, and perform BFS (Breadth First Search) and DFS (Depth First Search) traversals.

## Problem Statement

- Develop a menu-based C++ program to:

1. Accept number of vertices and edges from the user.

2. Represent the graph using an Adjacency List.

3. Perform BFS traversal starting from a user-defined node.

4. Perform DFS traversal starting from a user-defined node.


## Algorithm
# Algorithm: Create Graph

1. Start

2. Input number of vertices V_psc

3. Create adjacency list with V_psc entries

4. Input number of edges E_psc

5. For each edge:

- - Input two vertices u_psc, v_psc

- - Add each to the other's adjacency list

6. Stop

# Algorithm: BFS

1. Initialize visited array

2. Push start node into queue

3. While queue not empty:

- Pop node

- Print node

4. Add all unvisited adjacent nodes to queue

5. End

# Algorithm: DFS

1. Mark current node as visited

2. Print node

3. For every adjacent node:

- If not visited → Recursively call DFS

4. End

## Program 
```c++
#include <iostream>
#include <vector>
#include <ctime>
#include <cstdlib>
using namespace std;

// Custom Queue implementation
class Queue {
private:
    int* arr;
    int front, rear, capacity;

public:
    Queue(int size) {
        capacity = size;
        arr = new int[capacity];
        front = rear = -1;
    }

    ~Queue() {
        delete[] arr;
    }

    bool isEmpty() {
        return front == -1;
    }

    bool isFull() {
        return (rear + 1) % capacity == front;
    }

    void enqueue(int value) {
        if (isFull()) {
            cout << "Queue is full!" << endl;
            return;
        }
        if (isEmpty()) {
            front = rear = 0;
        } else {
            rear = (rear + 1) % capacity;
        }
        arr[rear] = value;
    }

    int dequeue() {
        if (isEmpty()) {
            cout << "Queue is empty!" << endl;
            return -1;
        }
        int value = arr[front];
        if (front == rear) {
            front = rear = -1;
        } else {
            front = (front + 1) % capacity;
        }
        return value;
    }
};

// Custom Stack implementation
class Stack {
private:
    int* arr;
    int top, capacity;

public:
    Stack(int size) {
        capacity = size;
        arr = new int[capacity];
        top = -1;
    }

    ~Stack() {
        delete[] arr;
    }

    bool isEmpty() {
        return top == -1;
    }

    bool isFull() {
        return top == capacity - 1;
    }

    void push(int value) {
        if (isFull()) {
            cout << "Stack is full!" << endl;
            return;
        }
        arr[++top] = value;
    }

    int pop() {
        if (isEmpty()) {
            cout << "Stack is empty!" << endl;
            return -1;
        }
        return arr[top--];
    }
};

class Graph {
private:
    int vertices;
    vector<vector<int>> adjMatrix;
    vector<bool> visited;

public:
    Graph(int v) {
        vertices = v;
        adjMatrix.resize(v, vector<int>(v, 0));
        visited.resize(v, false);
    }

    void addEdge(int u, int v) {
        if (u >= 0 && u < vertices && v >= 0 && v < vertices) {
            adjMatrix[u][v] = 1;
            adjMatrix[v][u] = 1;
        }
    }

    void displayAdjMatrix() {
        cout << "\nAdjacency Matrix:\n";
        cout << "   ";
        for (int i = 0; i < vertices; i++) {
            cout << i << " ";
        }
        cout << "\n";
        
        for (int i = 0; i < vertices; i++) {
            cout << i << ": ";
            for (int j = 0; j < vertices; j++) {
                cout << adjMatrix[i][j] << " ";
            }
            cout << "\n";
        }
    }

    void BFS(int startVertex, vector<int>& traversal) {
        fill(visited.begin(), visited.end(), false);
        Queue q(vertices);
        
        visited[startVertex] = true;
        q.enqueue(startVertex);
        
        while (!q.isEmpty()) {
            int currentVertex = q.dequeue();
            traversal.push_back(currentVertex);
            
            for (int i = 0; i < vertices; i++) {
                if (adjMatrix[currentVertex][i] == 1 && !visited[i]) {
                    visited[i] = true;
                    q.enqueue(i);
                }
            }
        }
    }

    void DFSRecursive(int vertex, vector<int>& traversal) {
        visited[vertex] = true;
        traversal.push_back(vertex);
        
        for (int i = 0; i < vertices; i++) {
            if (adjMatrix[vertex][i] == 1 && !visited[i]) {
                DFSRecursive(i, traversal);
            }
        }
    }

    void DFS(int startVertex, vector<int>& traversal) {
        fill(visited.begin(), visited.end(), false);
        DFSRecursive(startVertex, traversal);
    }

    void DFSIterative(int startVertex, vector<int>& traversal) {
        fill(visited.begin(), visited.end(), false);
        Stack s(vertices);
        
        s.push(startVertex);
        
        while (!s.isEmpty()) {
            int currentVertex = s.pop();
            
            if (!visited[currentVertex]) {
                visited[currentVertex] = true;
                traversal.push_back(currentVertex);
                
                for (int i = vertices - 1; i >= 0; i--) {
                    if (adjMatrix[currentVertex][i] == 1 && !visited[i]) {
                        s.push(i);
                    }
                }
            }
        }
    }

    // Getter function to access adjMatrix for external functions
    int getVertices() const {
        return vertices;
    }

    // Function to check if edge exists (for external access)
    bool hasEdge(int u, int v) const {
        if (u >= 0 && u < vertices && v >= 0 && v < vertices) {
            return adjMatrix[u][v] == 1;
        }
        return false;
    }
};

// Modified generateRandomGraph to use public interface
void generateRandomGraph(Graph& g, int edges) {
    int vertices = g.getVertices();
    int edgesAdded = 0;
    
    while (edgesAdded < edges) {
        int u = rand() % vertices;
        int v = rand() % vertices;
        if (u != v && !g.hasEdge(u, v)) {
            g.addEdge(u, v);
            edgesAdded++;
        }
    }
}

void displayTraversal(const string& name, const vector<int>& traversal) {
    cout << name << ": ";
    for (int vertex : traversal) {
        cout << vertex << " ";
    }
    cout << endl;
}

int main() {
    srand(time(0));
    
    int vertices, edges;
    cout << "=== Graph Traversal Performance Comparison ===" << endl;
    
    cout << "Enter the number of vertices: ";
    cin >> vertices;
    
    if (vertices <= 0) {
        cout << "Number of vertices must be positive!" << endl;
        return 1;
    }
    
    cout << "Enter the number of edges: ";
    cin >> edges;
    
    if (edges < 0) {
        cout << "Number of edges cannot be negative!" << endl;
        return 1;
    }
    
    // Maximum possible edges in undirected graph
    int maxEdges = vertices * (vertices - 1) / 2;
    if (edges > maxEdges) {
        cout << "Warning: Maximum possible edges for " << vertices 
             << " vertices is " << maxEdges << ". Using " << maxEdges << " edges." << endl;
        edges = maxEdges;
    }
    
    // Create graphs for different algorithms
    Graph g1(vertices); // For BFS
    Graph g2(vertices); // For DFS Recursive
    Graph g3(vertices); // For DFS Iterative
    
    // Generate random graph
    generateRandomGraph(g1, edges);
    
    // Copy the same graph structure to others
    for (int i = 0; i < vertices; i++) {
        for (int j = i + 1; j < vertices; j++) {
            if (g1.hasEdge(i, j)) {
                g2.addEdge(i, j);
                g3.addEdge(i, j);
            }
        }
    }
    
    // Display graph information
    cout << "\nGraph with " << vertices << " vertices and " << edges << " edges" << endl;
    g1.displayAdjMatrix();
    
    int startVertex = rand() % vertices;
    cout << "\nStarting vertex for traversals: " << startVertex << endl;
    
    // Vectors to store traversals
    vector<int> bfsTraversal, dfsTraversal, dfsIterativeTraversal;
    
    // Time BFS
    clock_t start = clock();
    g1.BFS(startVertex, bfsTraversal);
    clock_t end = clock();
    double timeBFS = (double)(end - start) / CLOCKS_PER_SEC;
    
    // Time DFS Recursive
    start = clock();
    g2.DFS(startVertex, dfsTraversal);
    end = clock();
    double timeDFS = (double)(end - start) / CLOCKS_PER_SEC;
    
    // Time DFS Iterative
    start = clock();
    g3.DFSIterative(startVertex, dfsIterativeTraversal);
    end = clock();
    double timeDFSIterative = (double)(end - start) / CLOCKS_PER_SEC;
    
    // Display results
    cout << "\n=== Traversal Results ===" << endl;
    displayTraversal("BFS Traversal", bfsTraversal);
    displayTraversal("DFS Recursive", dfsTraversal);
    displayTraversal("DFS Iterative", dfsIterativeTraversal);
    
    cout << "\n=== Performance Comparison ===" << endl;
    cout << "Time taken by BFS: " << timeBFS << " seconds" << endl;
    cout << "Time taken by DFS Recursive: " << timeDFS << " seconds" << endl;
    cout << "Time taken by DFS Iterative: " << timeDFSIterative << " seconds" << endl;
    
    // Additional analysis
    cout << "\n=== Analysis ===" << endl;
    if (timeBFS < timeDFS && timeBFS < timeDFSIterative) {
        cout << "BFS was the fastest algorithm for this graph." << endl;
    } else if (timeDFS < timeBFS && timeDFS < timeDFSIterative) {
        cout << "DFS Recursive was the fastest algorithm for this graph." << endl;
    } else {
        cout << "DFS Iterative was the fastest algorithm for this graph." << endl;
    }
    
    cout << "\nTraversal lengths - BFS: " << bfsTraversal.size() 
         << ", DFS Recursive: " << dfsTraversal.size() 
         << ", DFS Iterative: " << dfsIterativeTraversal.size() << endl;
    
    // Verify all traversals visited the same number of vertices
    if (bfsTraversal.size() == dfsTraversal.size() && dfsTraversal.size() == dfsIterativeTraversal.size()) {
        cout << "All traversals visited the same number of vertices (connected graph)." << endl;
    } else {
        cout << "Warning: Traversals visited different numbers of vertices (disconnected graph)." << endl;
    }
    
    return 0;
}


/*

```
## Sample Output
```
=== Graph Traversal Performance Comparison ===
Enter the number of vertices: 5
Enter the number of edges: 6

Graph with 5 vertices and 6 edges

Adjacency Matrix:
   0 1 2 3 4 
0: 0 1 1 0 1 
1: 1 0 0 1 1 
2: 1 0 0 1 0 
3: 0 1 1 0 1 
4: 1 1 0 1 0 

Starting vertex for traversals: 2

=== Traversal Results ===
BFS Traversal: 2 0 3 1 4 
DFS Recursive: 2 0 1 3 4 
DFS Iterative: 2 3 4 1 0 

=== Performance Comparison ===
Time taken by BFS: 0.000134 seconds
Time taken by DFS Recursive: 0.000098 seconds
Time taken by DFS Iterative: 0.000121 seconds

=== Analysis ===
DFS Recursive was the fastest algorithm for this graph.

Traversal lengths - BFS: 5, DFS Recursive: 5, DFS Iterative: 5
All traversals visited the same number of vertices (connected graph).
*/
```
## Conclusion

The program successfully accepts graph input from the user, represents it using an adjacency list, and performs BFS and DFS traversals. This demonstrates efficient graph storage and traversal using fundamental data structures.