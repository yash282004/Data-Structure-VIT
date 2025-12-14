## Assignment 4

*Title*
Write a Program to implement Kruskal’s algorithm to find the minimum spanning tree of a user defined graph. Use Adjacency List to represent a graph.

## Author:

**Yash Santosh Khandagale**

##  AIM

To write a C++ program to create a user-defined graph using adjacency list representation and apply Kruskal’s algorithm to find the Minimum Spanning Tree (MST).

## THEORY
- Graph

- A graph G(V, E) consists of a set of vertices and edges.

- Minimum Spanning Tree (MST)

- An MST of a connected weighted graph is a subset of edges that:

- connects all vertices,

- has no cycles,

- and has minimum total cost.

- Kruskal’s Algorithm (Greedy Algorithm)

- Kruskal's algorithm sorts all edges in ascending order of weight and adds them to the MST if they do not form a cycle.

## KRUSKAL’S ALGORITHM

- Sort all edges in non-decreasing order of weight.

- Initialize MST as empty.

- For each edge (u, v):

- If including it doesn't form a cycle → include it in MST.

- Use Disjoint Set / Union-Find to detect cycles.

- Repeat until MST contains (V−1) edges.

##  ADJACENCY LIST REPRESENTATION

- We store graph edges using:

- vector<pair<int, int>> adj[V];

- Each entry contains:
→ (neighbor, weight)

- We also maintain a list of edges:

- vector<Edge> edges;

##  C++ PROGRAM 
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <climits>
using namespace std;

// Structure to represent an edge in the graph
struct Edge {
    int source;
    int destination;
    int weight;
    
    Edge(int src, int dest, int w) : source(src), destination(dest), weight(w) {}
};

// Structure for Union-Find (Disjoint Set Union)
class UnionFind {
private:
    vector<int> parent;
    vector<int> rank;
    
public:
    UnionFind(int n) {
        parent.resize(n);
        rank.resize(n, 0);
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }
    }
    
    // Find with path compression
    int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);
        }
        return parent[x];
    }
    
    // Union by rank
    void unite(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        
        if (rootX != rootY) {
            if (rank[rootX] < rank[rootY]) {
                parent[rootX] = rootY;
            } else if (rank[rootX] > rank[rootY]) {
                parent[rootY] = rootX;
            } else {
                parent[rootY] = rootX;
                rank[rootX]++;
            }
        }
    }
    
    // Check if two elements are in the same set
    bool connected(int x, int y) {
        return find(x) == find(y);
    }
};

class Graph {
private:
    int vertices;
    int edges;
    vector<vector<pair<int, int>>> adjList; // adjacency list: vertex -> (neighbor, weight)
    vector<Edge> edgeList;
    
public:
    Graph(int v) {
        vertices = v;
        edges = 0;
        adjList.resize(v);
    }
    
    // Function to add an edge to the graph
    void addEdge(int u, int v, int weight) {
        if (u >= 0 && u < vertices && v >= 0 && v < vertices) {
            adjList[u].push_back({v, weight});
            adjList[v].push_back({u, weight}); // Undirected graph
            edgeList.push_back(Edge(u, v, weight));
            edges++;
        }
    }
    
    // Function to display the adjacency list
    void displayAdjList() {
        cout << "\nAdjacency List:\n";
        for (int i = 0; i < vertices; i++) {
            cout << "Vertex " << i << ": ";
            for (const auto& neighbor : adjList[i]) {
                cout << "-> (" << neighbor.first << ", " << neighbor.second << ") ";
            }
            cout << endl;
        }
    }
    
    // Function to display all edges
    void displayAllEdges() {
        cout << "\nAll Edges in the Graph:\n";
        cout << "Source\tDestination\tWeight\n";
        for (const Edge& edge : edgeList) {
            cout << edge.source << "\t" << edge.destination << "\t\t" << edge.weight << endl;
        }
    }
    
    // Kruskal's algorithm to find Minimum Spanning Tree
    void kruskalMST() {
        // Sort all edges by weight in non-decreasing order
        vector<Edge> sortedEdges = edgeList;
        sort(sortedEdges.begin(), sortedEdges.end(), 
             [](const Edge& a, const Edge& b) {
                 return a.weight < b.weight;
             });
        
        UnionFind uf(vertices);
        vector<Edge> mstEdges;
        int totalWeight = 0;
        int edgesAdded = 0;
        
        cout << "\nKruskal's Algorithm Execution:\n";
        cout << "==============================\n";
        cout << "Step\tEdge\t\tWeight\tAction\n";
        
        int step = 1;
        for (const Edge& edge : sortedEdges) {
            int u = edge.source;
            int v = edge.destination;
            int weight = edge.weight;
            
            if (!uf.connected(u, v)) {
                uf.unite(u, v);
                mstEdges.push_back(edge);
                totalWeight += weight;
                edgesAdded++;
                
                cout << step++ << "\t(" << u << "-" << v << ")\t\t" << weight << "\tAdded to MST\n";
                
                if (edgesAdded == vertices - 1) {
                    break; // MST has V-1 edges
                }
            } else {
                cout << step++ << "\t(" << u << "-" << v << ")\t\t" << weight << "\tSkip (forms cycle)\n";
            }
        }
        
        // Print the final MST
        printMST(mstEdges, totalWeight);
        
        // Check if MST is complete
        if (edgesAdded != vertices - 1) {
            cout << "\nWarning: Graph is disconnected. MST is not complete.\n";
        }
    }
    
    // Function to print the constructed MST
    void printMST(const vector<Edge>& mstEdges, int totalWeight) {
        cout << "\nMinimum Spanning Tree (Kruskal's Algorithm):\n";
        cout << "===========================================\n";
        cout << "Edge \t\tWeight\n";
        
        for (const Edge& edge : mstEdges) {
            cout << edge.source << " - " << edge.destination << " \t\t" << edge.weight << endl;
        }
        
        cout << "Total weight of MST: " << totalWeight << endl;
    }
    
    // Function to get number of vertices
    int getVertices() const {
        return vertices;
    }
    
    // Function to get number of edges
    int getEdges() const {
        return edges;
    }
};

// Function to create a graph based on user input
Graph createUserGraph() {
    int vertices, edges;
    
    cout << "=== Kruskal's Algorithm - Minimum Spanning Tree ===\n";
    
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
        
        if (u == v) {
            cout << "Self-loops are not allowed! Skipping this edge.\n";
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
    Graph g(6);
    
    // Adding edges for the sample graph
    g.addEdge(0, 1, 4);
    g.addEdge(0, 2, 4);
    g.addEdge(1, 2, 2);
    g.addEdge(2, 3, 3);
    g.addEdge(2, 5, 2);
    g.addEdge(2, 4, 4);
    g.addEdge(3, 4, 3);
    g.addEdge(5, 4, 3);
    
    return g;
}

// Function to create a disconnected graph for testing
Graph createDisconnectedGraph() {
    cout << "\nCreating disconnected graph for testing...\n";
    Graph g(6);
    
    // Component 1: vertices 0,1,2
    g.addEdge(0, 1, 1);
    g.addEdge(1, 2, 2);
    g.addEdge(0, 2, 3);
    
    // Component 2: vertices 3,4,5
    g.addEdge(3, 4, 4);
    g.addEdge(4, 5, 5);
    g.addEdge(3, 5, 6);
    
    return g;
}

int main() {
    int choice;
    
    cout << "Choose graph input method:\n";
    cout << "1. User-defined graph\n";
    cout << "2. Use sample graph\n";
    cout << "3. Use disconnected graph (for testing)\n";
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
            g = createDisconnectedGraph();
            break;
        default:
            cout << "Invalid choice! Using sample graph.\n";
            g = createSampleGraph();
    }
    
    // Display the graph information
    cout << "\nGraph Information:\n";
    cout << "Vertices: " << g.getVertices() << endl;
    cout << "Edges: " << g.getEdges() << endl;
    
    // Display the adjacency list
    g.displayAdjList();
    
    // Display all edges
    g.displayAllEdges();
    
    // Execute Kruskal's algorithm
    g.kruskalMST();
    
    return 0;
}
```
## SAMPLE INPUT
```
=== Kruskal's Algorithm - Minimum Spanning Tree ===
Choose graph input method:
1. User-defined graph
2. Use sample graph
3. Use disconnected graph (for testing)
Enter your choice (1, 2, or 3): 2

Creating sample graph for demonstration...

Graph Information:
Vertices: 6
Edges: 8

Adjacency List:
Vertex 0: -> (1, 4) -> (2, 4) 
Vertex 1: -> (0, 4) -> (2, 2) 
Vertex 2: -> (0, 4) -> (1, 2) -> (3, 3) -> (5, 2) -> (4, 4) 
Vertex 3: -> (2, 3) -> (4, 3) 
Vertex 4: -> (2, 4) -> (3, 3) -> (5, 3) 
Vertex 5: -> (2, 2) -> (4, 3) 

All Edges in the Graph:
Source  Destination Weight
0       1           4
0       2           4
1       2           2
2       3           3
2       5           2
2       4           4
3       4           3
5       4           3

Kruskal's Algorithm Execution:
==============================
Step    Edge            Weight  Action
1       (1-2)           2       Added to MST
2       (2-5)           2       Added to MST
3       (2-3)           3       Added to MST
4       (3-4)           3       Added to MST
5       (0-1)           4       Added to MST
6       (0-2)           4       Skip (forms cycle)
7       (2-4)           4       Skip (forms cycle)
8       (5-4)           3       Skip (forms cycle)

Minimum Spanning Tree (Kruskal's Algorithm):
===========================================
Edge            Weight
1 - 2           2
2 - 5           2
2 - 3           3
3 - 4           3
0 - 1           4
Total weight of MST: 14
```
##  CONCLUSION

- Kruskal’s algorithm efficiently finds the Minimum Spanning Tree (MST) by using:

- - edge-sorting,

- - greedy strategy,

- - and Disjoint Set (Union-Find) to avoid cycles.