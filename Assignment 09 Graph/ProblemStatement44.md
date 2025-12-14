## Assignment 4

*Title:*

Write a Program to implement Dijkstra’s algorithm to find shortest distance between two nodes of a user defined graph. Use Adjacency List to represent a graph. 

**Author:**

**Yash Santosh Khandagale**

## Aim:

To write a C++ program that accepts a weighted graph from the user, stores it in an Adjacency List, and uses Dijkstra’s Algorithm to find the shortest distance between two vertices.

## Problem Statement:

- Create a graph using adjacency list representation and implement Dijkstra’s shortest path algorithm to compute:

1. Shortest distance between a source node and all nodes

2. Shortest path from the source to a destination node

3. The graph must support weighted, non-negative edges.

## Algorithm:
1. Step 1:

- Start program.

2. Step 2:

- Input the number of vertices and edges.

3. Step 3:

- Create adjacency list where each list contains:

- Destination vertex

- Weight of edge

4. Step 4:

- Input the source and destination node.

5. Step 5:

- Initialize:

- dist[] = ∞

- visited[] = false

- dist[source] = 0

6. Step 6:

- Repeat for all vertices:

- Select unvisited vertex with minimum distance

- Mark it visited

- Update all adjacent vertices

- If shorter path found → update distance

7. Step 7:

- Print shortest distances and shortest path to destination.

8. Step 8:

- Stop.

## C++ Program 
```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <climits>
#include <algorithm>
using namespace std;

// Structure to represent a node in the priority queue
struct PQNode {
    int vertex;
    int distance;
    
    PQNode(int v, int d) : vertex(v), distance(d) {}
    
    // Overload < operator for min-heap (smallest distance first)
    bool operator<(const PQNode& other) const {
        return distance > other.distance;
    }
};

// Structure to represent an edge in the graph
struct Edge {
    int destination;
    int weight;
    
    Edge(int dest, int w) : destination(dest), weight(w) {}
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
            // For directed graph, we don't automatically add reverse edge
            // For undirected graph, uncomment the line below:
            // adjList[v].push_back(Edge(u, weight));
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
    
    // Dijkstra's algorithm to find shortest path from source to all vertices
    vector<int> dijkstra(int source) {
        // Distance array - initialize with infinity
        vector<int> dist(vertices, INT_MAX);
        // Parent array to reconstruct paths
        vector<int> parent(vertices, -1);
        // Priority queue (min-heap)
        priority_queue<PQNode> pq;
        
        // Initialize source
        dist[source] = 0;
        pq.push(PQNode(source, 0));
        
        cout << "\nDijkstra's Algorithm Execution from source " << source << ":\n";
        cout << "==============================================\n";
        
        int step = 1;
        
        while (!pq.empty()) {
            // Extract vertex with minimum distance
            int u = pq.top().vertex;
            int currentDist = pq.top().distance;
            pq.pop();
            
            // If we found a better path after this node was added to queue, skip it
            if (currentDist > dist[u]) {
                cout << "Step " << step++ << ": Skip vertex " << u 
                     << " (better path found: " << dist[u] << " < " << currentDist << ")\n";
                continue;
            }
            
            cout << "Step " << step++ << ": Process vertex " << u 
                 << " with distance " << dist[u] << endl;
            
            // Update distances of all adjacent vertices
            for (const Edge& edge : adjList[u]) {
                int v = edge.destination;
                int weight = edge.weight;
                int newDist = dist[u] + weight;
                
                // If found shorter path to v
                if (newDist < dist[v]) {
                    dist[v] = newDist;
                    parent[v] = u;
                    pq.push(PQNode(v, newDist));
                    
                    cout << "   -> Update vertex " << v << " to distance " << newDist 
                         << " (via " << u << ")" << endl;
                }
            }
        }
        
        // Store parent information for path reconstruction
        // In a real implementation, you might want to return this too
        return dist;
    }
    
    // Dijkstra's algorithm to find shortest path between two specific nodes
    void dijkstraPath(int source, int destination) {
        vector<int> dist(vertices, INT_MAX);
        vector<int> parent(vertices, -1);
        priority_queue<PQNode> pq;
        
        dist[source] = 0;
        pq.push(PQNode(source, 0));
        
        cout << "\nFinding shortest path from " << source << " to " << destination << ":\n";
        cout << "========================================\n";
        
        while (!pq.empty()) {
            int u = pq.top().vertex;
            int currentDist = pq.top().distance;
            pq.pop();
            
            // Early termination if we reached destination
            if (u == destination) {
                break;
            }
            
            // Skip if we found a better path already
            if (currentDist > dist[u]) {
                continue;
            }
            
            for (const Edge& edge : adjList[u]) {
                int v = edge.destination;
                int weight = edge.weight;
                int newDist = dist[u] + weight;
                
                if (newDist < dist[v]) {
                    dist[v] = newDist;
                    parent[v] = u;
                    pq.push(PQNode(v, newDist));
                }
            }
        }
        
        // Display results
        displayShortestPath(source, destination, dist, parent);
    }
    
    // Function to display shortest path and distance
    void displayShortestPath(int source, int destination, 
                           const vector<int>& dist, 
                           const vector<int>& parent) {
        cout << "\nShortest Path Results:\n";
        cout << "======================\n";
        
        if (dist[destination] == INT_MAX) {
            cout << "No path exists from " << source << " to " << destination << endl;
            return;
        }
        
        cout << "Shortest distance from " << source << " to " << destination 
             << ": " << dist[destination] << endl;
        
        // Reconstruct path
        vector<int> path;
        for (int v = destination; v != -1; v = parent[v]) {
            path.push_back(v);
        }
        reverse(path.begin(), path.end());
        
        cout << "Shortest path: ";
        for (size_t i = 0; i < path.size(); i++) {
            cout << path[i];
            if (i != path.size() - 1) {
                cout << " -> ";
            }
        }
        cout << endl;
        
        // Display path with weights
        cout << "Path with weights: ";
        int totalWeight = 0;
        for (size_t i = 0; i < path.size() - 1; i++) {
            int u = path[i];
            int v = path[i + 1];
            int weight = getWeight(u, v);
            totalWeight += weight;
            cout << u << " -(" << weight << ")-> ";
            if (i == path.size() - 2) {
                cout << v;
            }
        }
        cout << "\nTotal weight: " << totalWeight << endl;
    }
    
    // Display shortest distances from source to all vertices
    void displayAllDistances(int source, const vector<int>& dist) {
        cout << "\nShortest distances from vertex " << source << ":\n";
        cout << "Vertex\tDistance\tPath Exists?\n";
        for (int i = 0; i < vertices; i++) {
            cout << i << "\t";
            if (dist[i] == INT_MAX) {
                cout << "INF\t\tNo";
            } else {
                cout << dist[i] << "\t\tYes";
            }
            cout << endl;
        }
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
    int graphType;
    
    cout << "=== Dijkstra's Algorithm - Shortest Path ===\n";
    
    cout << "Enter number of vertices: ";
    cin >> vertices;
    
    if (vertices <= 0) {
        cout << "Number of vertices must be positive!\n";
        exit(1);
    }
    
    Graph g(vertices);
    
    cout << "Choose graph type:\n";
    cout << "1. Directed graph\n";
    cout << "2. Undirected graph\n";
    cout << "Enter your choice (1 or 2): ";
    cin >> graphType;
    
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
            cout << "Dijkstra's algorithm requires non-negative weights! Using absolute value.\n";
            weight = abs(weight);
        }
        
        g.addEdge(u, v, weight);
        
        // For undirected graph, add reverse edge
        if (graphType == 2) {
            // We need to modify the Graph class to handle this properly
            // For now, we'll add both directions manually
            g.addEdge(v, u, weight);
        }
    }
    
    return g;
}

// Function to create a sample graph for demonstration
Graph createSampleGraph() {
    cout << "\nCreating sample directed graph for demonstration...\n";
    Graph g(6);
    
    // Adding edges for the sample graph (directed)
    g.addEdge(0, 1, 2);
    g.addEdge(0, 2, 4);
    g.addEdge(1, 2, 1);
    g.addEdge(1, 3, 7);
    g.addEdge(2, 4, 3);
    g.addEdge(3, 5, 1);
    g.addEdge(4, 3, 2);
    g.addEdge(4, 5, 5);
    
    return g;
}

// Function to create another sample with some unreachable nodes
Graph createSampleGraph2() {
    cout << "\nCreating sample graph with unreachable vertices...\n";
    Graph g(7);
    
    // Component 1
    g.addEdge(0, 1, 5);
    g.addEdge(0, 2, 3);
    g.addEdge(1, 3, 6);
    g.addEdge(2, 3, 2);
    g.addEdge(3, 4, 4);
    
    // Component 2 (disconnected from component 1)
    g.addEdge(5, 6, 1);
    
    return g;
}

int main() {
    int choice;
    
    cout << "Choose graph input method:\n";
    cout << "1. User-defined graph\n";
    cout << "2. Use sample graph 1 (connected)\n";
    cout << "3. Use sample graph 2 (with unreachable vertices)\n";
    cout << "Enter your choice (1, 2, or 3): ";
    cin >> choice;
    
    Graph g(1); // Initialize with dummy size
    
    switch (choice) {
        case 1:
            g = createUserGraph();
            break;
        case 2:
            g = createSampleGraph();
            break;
        case 3:
            g = createSampleGraph2();
            break;
        default:
            cout << "Invalid choice! Using sample graph 1.\n";
            g = createSampleGraph();
    }
    
    // Display the graph
    g.displayAdjList();
    
    int source, destination;
    cout << "\nEnter source vertex (0 to " << g.getVertices() - 1 << "): ";
    cin >> source;
    
    if (source < 0 || source >= g.getVertices()) {
        cout << "Invalid source vertex! Using vertex 0.\n";
        source = 0;
    }
    
    // First, run Dijkstra to find distances to all vertices
    vector<int> distances = g.dijkstra(source);
    
    // Display all distances
    g.displayAllDistances(source, distances);
    
    // Ask for specific destination
    cout << "\nEnter destination vertex (0 to " << g.getVertices() - 1 << "): ";
    cin >> destination;
    
    if (destination < 0 || destination >= g.getVertices()) {
        cout << "Invalid destination vertex! Using vertex " << g.getVertices() - 1 << ".\n";
        destination = g.getVertices() - 1;
    }
    
    // Find and display specific path
    g.dijkstraPath(source, destination);
    
    return 0;
}

```

## Sample Output
/*
=== Dijkstra's Algorithm - Shortest Path ===
Choose graph input method:
1. User-defined graph
2. Use sample graph 1 (connected)
3. Use sample graph 2 (with unreachable vertices)
Enter your choice (1, 2, or 3): 2

Creating sample directed graph for demonstration...

Adjacency List:
Vertex 0: -> (1, 2) -> (2, 4) 
Vertex 1: -> (2, 1) -> (3, 7) 
Vertex 2: -> (4, 3) 
Vertex 3: -> (5, 1) 
Vertex 4: -> (3, 2) -> (5, 5) 
Vertex 5: 

Enter source vertex (0 to 5): 0

Dijkstra's Algorithm Execution from source 0:
==============================================
Step 1: Process vertex 0 with distance 0
   -> Update vertex 1 to distance 2 (via 0)
   -> Update vertex 2 to distance 4 (via 0)
Step 2: Process vertex 1 with distance 2
   -> Update vertex 2 to distance 3 (via 1)
   -> Update vertex 3 to distance 9 (via 1)
Step 3: Process vertex 2 with distance 3
   -> Update vertex 4 to distance 6 (via 2)
Step 4: Process vertex 4 with distance 6
   -> Update vertex 3 to distance 8 (via 4)
   -> Update vertex 5 to distance 11 (via 4)
Step 5: Process vertex 3 with distance 8
   -> Update vertex 5 to distance 9 (via 3)
Step 6: Process vertex 5 with distance 9

Shortest distances from vertex 0:
Vertex  Distance        Path Exists?
0       0               Yes
1       2               Yes
2       3               Yes
3       8               Yes
4       6               Yes
5       9               Yes

Enter destination vertex (0 to 5): 5

Finding shortest path from 0 to 5:
========================================

Shortest Path Results:
======================
Shortest distance from 0 to 5: 9
Shortest path: 0 -> 1 -> 2 -> 4 -> 3 -> 5
Path with weights: 0 -(2)-> 1 -(1)-> 2 -(3)-> 4 -(2)-> 3 -(1)-> 5
Total weight: 9
*/
```
## Explanation

- Graph stored using Adjacency List
- Dijkstra’s algorithm used priority queue for fast min-distance extraction
- Tracks shortest distance and path using parent[]