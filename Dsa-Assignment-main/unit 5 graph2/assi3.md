## ASSIGNMENT 3
*Title:*

Write a Program to implement Prim’s algorithm to find minimum
spanning tree of a user defined graph. Use Adjacency List to represent a graph.

## Author:

**Yash Santosh Khandagale**

## Aim:

To write a C++ program to find the Minimum Spanning Tree (MST) of a user-defined graph using Prim’s Algorithm with an Adjacency List representation.

## Problem Statement:

- Develop a C++ program that:

1. Accepts vertices and weighted edges from the user

2. Stores the graph in Adjacency List form

3. Applies Prim’s Algorithm to compute MST

4. Displays the edges and minimum total cost

## Theory
- Graph (Weighted Undirected)

- A graph where each edge carries a weight.

- Adjacency List

- A list of linked or vector-based lists representing each node and its neighbors.

- Efficient for sparse graphs.

- Prim’s Algorithm

- Greedy algorithm for finding MST

- Starts from any single node

- Repeatedly selects the minimum weight edge connecting a visited node to an unvisited node

## Algorithm: Prim’s Algorithm

1. Start

2. Read total vertices V_psc

3. Create adjacency list adj_psc

4. Take edges (u_psc, v_psc, w_psc)

5. Select start node

6. Push all its adjacent edges into priority queue

7. While queue is not empty:

8. Pick smallest edge

9. If destination is unvisited, add to MST

10. Push its edges

11. Repeat until MST has (V_psc - 1) edges

12. Display MST and total cost

13. Stop

## C++ Program 
```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <climits>
#include <algorithm>
using namespace std;

// Structure to represent an edge in the graph
struct Edge {
    int destination;
    int weight;
    Edge(int dest, int w) : destination(dest), weight(w) {}
};

// Structure to represent a node in the priority queue
struct PQNode {
    int vertex;
    int key; // key value used for priority queue
    int parent;
    
    PQNode(int v, int k, int p = -1) : vertex(v), key(k), parent(p) {}
    
    // Overload < operator for min-heap
    bool operator<(const PQNode& other) const {
        return key > other.key; // Min-heap based on key
    }
};

class Graph {
private:
    int vertices;
    vector<vector<Edge>> adjList;
    
public:
    Graph(int v) {
        vertices = v;
        adjList.resize(v);
    }
    
    // Function to add an edge to the graph
    void addEdge(int u, int v, int weight) {
        if (u >= 0 && u < vertices && v >= 0 && v < vertices) {
            adjList[u].push_back(Edge(v, weight));
            adjList[v].push_back(Edge(u, weight)); // Undirected graph
        }
    }
    
    // Function to display the adjacency list
    void displayAdjList() {
        cout << "\nAdjacency List:\n";
        for (int i = 0; i < vertices; i++) {
            cout << "Vertex " << i << ": ";
            for (const Edge& edge : adjList[i]) {
                cout << "-> (" << edge.destination << ", " << edge.weight << ") ";
            }
            cout << endl;
        }
    }
    
    // Prim's algorithm to find Minimum Spanning Tree
    void primMST(int startVertex) {
        // Arrays to store MST information
        vector<int> key(vertices, INT_MAX);    // Key values used to pick minimum weight edge
        vector<int> parent(vertices, -1);      // Array to store constructed MST
        vector<bool> inMST(vertices, false);   // To represent set of vertices included in MST
        
        // Priority queue (min-heap) to store vertices and their key values
        priority_queue<PQNode> pq;
        
        // Initialize starting vertex
        key[startVertex] = 0;
        pq.push(PQNode(startVertex, 0));
        
        cout << "\nPrim's Algorithm Execution:\n";
        cout << "===========================\n";
        
        int step = 1;
        int totalWeight = 0;
        
        while (!pq.empty()) {
            // Extract the vertex with minimum key value
            int u = pq.top().vertex;
            pq.pop();
            
            // If vertex is already in MST, skip it
            if (inMST[u]) continue;
            
            // Include vertex in MST
            inMST[u] = true;
            totalWeight += key[u];
            
            // Print the step
            if (parent[u] != -1) {
                cout << "Step " << step++ << ": Add edge (" << parent[u] 
                     << " - " << u << ") with weight " << key[u] << endl;
            } else {
                cout << "Step " << step++ << ": Start with vertex " << u << endl;
            }
            
            // Update key values and parent index of adjacent vertices
            for (const Edge& edge : adjList[u]) {
                int v = edge.destination;
                int weight = edge.weight;
                
                // If v is not in MST and weight of (u,v) is smaller than current key of v
                if (!inMST[v] && weight < key[v]) {
                    key[v] = weight;
                    parent[v] = u;
                    pq.push(PQNode(v, key[v]));
                }
            }
        }
        
        // Print the final MST
        printMST(parent, totalWeight);
    }
    
    // Function to print the constructed MST
    void printMST(const vector<int>& parent, int totalWeight) {
        cout << "\nMinimum Spanning Tree:\n";
        cout << "======================\n";
        cout << "Edge \t\tWeight\n";
        
        // Start from 1 because parent[0] is always -1
        for (int i = 1; i < vertices; i++) {
            if (parent[i] != -1) {
                cout << parent[i] << " - " << i << " \t\t" 
                     << getWeight(parent[i], i) << endl;
            }
        }
        
        cout << "Total weight of MST: " << totalWeight << endl;
    }
    
    // Helper function to get weight of an edge
    int getWeight(int u, int v) {
        for (const Edge& edge : adjList[u]) {
            if (edge.destination == v) {
                return edge.weight;
            }
        }
        return INT_MAX;
    }
    
    // Function to get number of vertices
    int getVertices() const {
        return vertices;
    }
};

// Function to create a graph based on user input
Graph createUserGraph() {
    int vertices, edges;
    
    cout << "=== Prim's Algorithm - Minimum Spanning Tree ===\n";
    
    cout << "Enter number of vertices: ";
    cin >> vertices;
    
    if (vertices <= 0) {
        cout << "Number of vertices must be positive!\n";
        exit(1);
    }
    
    Graph g(vertices);
    
    cout << "Enter number of edges: ";
    cin >> edges;
    
    if (edges < 0) {
        cout << "Number of edges cannot be negative!\n";
        exit(1);
    }
    
    cout << "\nEnter edges (format: u v weight):\n";
    for (int i = 0; i < edges; i++) {
        int u, v, weight;
        cout << "Edge " << i + 1 << ": ";
        cin >> u >> v >> weight;
        
        if (u < 0 || u >= vertices || v < 0 || v >= vertices) {
            cout << "Invalid vertices! Must be between 0 and " << vertices - 1 << endl;
            i--;
            continue;
        }
        
        if (weight < 0) {
            cout << "Weight cannot be negative! Using absolute value.\n";
            weight = abs(weight);
        }
        
        g.addEdge(u, v, weight);
    }
    
    return g;
}

// Function to create a sample graph for demonstration
Graph createSampleGraph() {
    cout << "\nCreating sample graph for demonstration...\n";
    Graph g(5);
    
    // Adding edges for the sample graph
    g.addEdge(0, 1, 2);
    g.addEdge(0, 3, 6);
    g.addEdge(1, 2, 3);
    g.addEdge(1, 3, 8);
    g.addEdge(1, 4, 5);
    g.addEdge(2, 4, 7);
    g.addEdge(3, 4, 9);
    
    return g;
}

int main() {
    int choice;
    
    cout << "Choose graph input method:\n";
    cout << "1. User-defined graph\n";
    cout << "2. Use sample graph\n";
    cout << "Enter your choice (1 or 2): ";
    cin >> choice;
    
    Graph g(1); // Initialize with dummy size
    
    if (choice == 1) {
        g = createUserGraph();
    } else if (choice == 2) {
        g = createSampleGraph();
    } else {
        cout << "Invalid choice! Using sample graph.\n";
        g = createSampleGraph();
    }
    
    // Display the graph
    g.displayAdjList();
    
    // Get starting vertex for Prim's algorithm
    int startVertex;
    cout << "\nEnter starting vertex for Prim's algorithm (0 to " << g.getVertices() - 1 << "): ";
    cin >> startVertex;
    
    if (startVertex < 0 || startVertex >= g.getVertices()) {
        cout << "Invalid starting vertex! Using vertex 0.\n";
        startVertex = 0;
    }
    
    // Execute Prim's algorithm
    g.primMST(startVertex);
    
    return 0;
}
```
## Sample Output
```
=== Prim's Algorithm - Minimum Spanning Tree ===
Choose graph input method:
1. User-defined graph
2. Use sample graph
Enter your choice (1 or 2): 2

Creating sample graph for demonstration...

Adjacency List:
Vertex 0: -> (1, 2) -> (3, 6) 
Vertex 1: -> (0, 2) -> (2, 3) -> (3, 8) -> (4, 5) 
Vertex 2: -> (1, 3) -> (4, 7) 
Vertex 3: -> (0, 6) -> (1, 8) -> (4, 9) 
Vertex 4: -> (1, 5) -> (2, 7) -> (3, 9) 

Enter starting vertex for Prim's algorithm (0 to 4): 0

Prim's Algorithm Execution:
===========================
Step 1: Start with vertex 0
Step 2: Add edge (0 - 1) with weight 2
Step 3: Add edge (1 - 2) with weight 3
Step 4: Add edge (1 - 4) with weight 5
Step 5: Add edge (0 - 3) with weight 6

Minimum Spanning Tree:
======================
Edge            Weight
0 - 1           2
1 - 2           3
0 - 3           6
1 - 4           5
Total weight of MST: 16
```
## Conclusion

The program successfully implements Prim’s Algorithm using an Adjacency List, generating the Minimum Spanning Tree and calculating its total cost. The adjacency list representation ensures efficient storage and traversal of sparse graphs.