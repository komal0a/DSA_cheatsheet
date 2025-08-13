#include <bits/stdc++.h>
using namespace std;

bool detectCycleBFS(int start, vector<vector<int>>& adj, vector<bool>& visited) {
    queue<pair<int,int>> q; 
    visited[start] = true;
    q.push({start, -1}); // {node, parent}

    while (!q.empty()) {
        int node = q.front().first;
        int parent = q.front().second;
        q.pop();

        for (auto neigh : adj[node]) {
            if (!visited[neigh]) {
                visited[neigh] = true;
                q.push({neigh, node});
            }
            else if (neigh != parent) {
                return true; // visited but not parent → cycle
            }
        }
    }
    return false;
}
//to find cycle in undirected graph
bool isCycle(int V, vector<vector<int>>& adj) {
    vector<bool> visited(V, false);
    for (int i = 0; i < V; i++) {
        if (!visited[i]) {
            if (detectCycleBFS(i, adj, visited)) 
                return true;
        }
    }
    return false;
}

#include <bits/stdc++.h>
using namespace std;

bool dfs(int node, int parent, vector<vector<int>>& adj, vector<bool>& visited) {
    visited[node] = true;

    for (auto neigh : adj[node]) {
        if (!visited[neigh]) {
            if (dfs(neigh, node, adj, visited))
                return true;
        }
        else if (neigh != parent) {
            return true; // visited but not parent → cycle
        }
    }
    return false;
}
//dfs to find cycle in undirected graph
bool isCycle(int V, vector<vector<int>>& adj) {
    vector<bool> visited(V, false);
    for (int i = 0; i < V; i++) {
        if (!visited[i]) {
            if (dfs(i, -1, adj, visited))
                return true;
        }
    }
    return false;
}

### Key Differences in Cycle Detection Approaches

| Approach        | Graph Type | Extra Data Structure        | Logic                                            |
|-----------------|------------|-----------------------------|--------------------------------------------------|
| **DFS**         | Directed   | `visited[]`, `pathVisited[]` | Detects back edges in the current recursion path |
| **BFS (Kahn)**  | Directed   | `indegree[]`, queue          | If topological sort misses nodes → cycle exists  |

## 1. DFS Cycle Detection in Directed Graph

```cpp
#include <bits/stdc++.h>
using namespace std;

bool dfs(int node, vector<vector<int>>& adj, vector<int>& visited, vector<int>& pathVisited) {
    visited[node] = 1;
    pathVisited[node] = 1;

    for (auto neigh : adj[node]) {
        if (!visited[neigh]) {
            if (dfs(neigh, adj, visited, pathVisited)) return true;
        }
        else if (pathVisited[neigh]) {
            return true; // back edge → cycle
        }
    }

    pathVisited[node] = 0; // remove from current path
    return false;
}

bool isCycleDFS(int V, vector<vector<int>>& adj) {
    vector<int> visited(V, 0), pathVisited(V, 0);
    for (int i = 0; i < V; i++) {
        if (!visited[i]) {
            if (dfs(i, adj, visited, pathVisited)) return true;
        }
    }
    return false;
}

int main() {
    int V = 4;
    vector<vector<int>> adj(V);
    adj[0] = {1};
    adj[1] = {2};
    adj[2] = {3};
    adj[3] = {1}; // cycle

    if (isCycleDFS(V, adj)) cout << "Cycle found\n";
    else cout << "No cycle\n";
}

#include <bits/stdc++.h>
using namespace std;

bool isCycleBFS(int V, vector<vector<int>>& adj) {
    vector<int> indegree(V, 0);

    for (int i = 0; i < V; i++) {
        for (auto neigh : adj[i]) {
            indegree[neigh]++;
        }
    }

    queue<int> q;
    for (int i = 0; i < V; i++) {
        if (indegree[i] == 0) q.push(i);
    }

    int count = 0;
    while (!q.empty()) {
        int node = q.front(); q.pop();
        count++;

        for (auto neigh : adj[node]) {
            indegree[neigh]--;
            if (indegree[neigh] == 0) q.push(neigh);
        }
    }

    return count != V; // if all nodes not processed → cycle
}

int main() {
    int V = 4;
    vector<vector<int>> adj(V);
    adj[0] = {1};
    adj[1] = {2};
    adj[2] = {3};
    adj[3] = {1}; // cycle

    if (isCycleBFS(V, adj)) cout << "Cycle found\n";
    else cout << "No cycle\n";
}
#include <bits/stdc++.h>
using namespace std;

void dfs(int node, vector<vector<int>>& adj, vector<bool>& visited, stack<int>& st) {
    visited[node] = true;
    for (auto neigh : adj[node]) {
        if (!visited[neigh]) {
            dfs(neigh, adj, visited, st);
        }
    }
    st.push(node);
}

vector<int> topoSortDFS(int V, vector<vector<int>>& adj) {
    vector<bool> visited(V, false);
    stack<int> st;

    for (int i = 0; i < V; i++) {
        if (!visited[i]) {
            dfs(i, adj, visited, st);
        }
    }

    vector<int> topo;
    while (!st.empty()) {
        topo.push_back(st.top());
        st.pop();
    }
    return topo;
}

#include<bits/stdc++.h>
using namespace std;

class Solution {
private: 
    bool dfs(int node, int col, int color[], vector<int> adj[]) {
        color[node] = col; 
        
        // traverse adjacent nodes
        for(auto it : adj[node]) {
            // if uncoloured
            if(color[it] == -1) {
                if(dfs(it, !col, color, adj) == false) return false; 
            }
            // if previously coloured and have the same colour
            else if(color[it] == col) {
                return false; 
            }
        }
        
        return true; 
    }
public:
	bool isBipartite(int V, vector<int>adj[]){
	    int color[V];
	    for(int i = 0;i<V;i++) color[i] = -1; 
	    
	    // for connected components
	    for(int i = 0;i<V;i++) {
	        if(color[i] == -1) {
	            if(dfs(i, 0, color, adj) == false) 
	                return false; 
	        }
	    }
	    return true; 
	}
};
class Solution {
public:
    bool bfs(vector<vector<int>>& graph,vector<int>&color,int node){
        queue<int>qt;
        qt.push(node);
        color[node]=1;

        while(!qt.empty()){
            int nod=qt.front();
            qt.pop();

        for(auto &it:graph[nod]){
            if(color[it]==-1){
                color[it]=!color[nod];
                qt.push(it);
            }else if(color[it]==color[nod]){
                return false;
            }
        }
        }
        return true;
    }
    bool isBipartite(vector<vector<int>>& graph) {
           int n=graph.size();
           vector<int>color(n,-1);
           for(int i=0;i<n;i++){
            if(color[i]==-1){
                if(!bfs(graph,color,i))return false;
            }
           }
           return true;
    }
};

class Solution {
public:
	void shortestDistance(vector<vector<int>>&matrix) {
        int n=matrix.size();
    
    
	for(int i=0; i<n;i++){
        for(int j=0; j<n;j++){
            if(matrix[i][j]==-1){
                adj[i][j]=1e9;
            }
            if(i==j)matrix[i][j]=0;
        }
    }

    for(int k=0; k<n;k++){
        for(int i=0; i<n;i++){
            for(int j=0;j<n;j++){
                adj[i][j]=min(matrix[i][j],matrix[i][k]+matrix[k][j]);
            }
        }
    }

    for (int i = 0; i < n; i++) {
			for (int j = 0; j < n; j++) {
				if (matrix[i][j] == 1e9) {
					matrix[i][j] = -1;
				}
			}
		}
	}
};

class Solution {
public:
	vector<int> bellman_ford(int V, vector<vector<int>>& edges, int S) {
        vector<int>dist(V,1e9);
        dist[S]=0;
            for(int j=0; j<V-1;j++){
                 for(auto& it:edges){
                    int a=it[0];
                    int b= it[1];
                    int w=it[2];
                    if(dist[a]!=1e9 && dist[b]>w+dist[a]){
                        dist[b]=w+dist[a];
                    }
            }
        }
        return dist;
	}
};
#include <bits/stdc++.h>
using namespace std;

class Solution
{
public:
    vector<int> shortestPath(int n, int m, vector<vector<int>> &edges)
    {
        // Create an adjacency list of pairs of the form node1 -> {node2, edge weight}
        // where the edge weight is the weight of the edge from node1 to node2.
        vector<pair<int, int>> adj[n + 1];
        for (auto it : edges)
        {
            adj[it[0]].push_back({it[1], it[2]});
            adj[it[1]].push_back({it[0], it[2]});
        }
        // Create a priority queue for storing the nodes along with distances 
        // in the form of a pair { dist, node }.
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int,int>>> pq;

        // Create a dist array for storing the updated distances and a parent array
        //for storing the nodes from where the current nodes represented by indices of
        // the parent array came from.
        vector<int> dist(n + 1, 1e9), parent(n + 1);
        for (int i = 1; i <= n; i++)
            parent[i] = i;

        dist[1] = 0;

        // Push the source node to the queue.
        pq.push({0, 1});
        while (!pq.empty())
        {
            // Topmost element of the priority queue is with minimum distance value.
            auto it = pq.top();
            pq.pop();
            int node = it.second;
            int dis = it.first;

            // Iterate through the adjacent nodes of the current popped node.
            for (auto it : adj[node])
            {
                int adjNode = it.first;
                int edW = it.second;

                // Check if the previously stored distance value is 
                // greater than the current computed value or not, 
                // if yes then update the distance value.
                if (dis + edW < dist[adjNode])
                {
                    dist[adjNode] = dis + edW;
                    pq.push({dis + edW, adjNode});

                    // Update the parent of the adjNode to the recent 
                    // node where it came from.
                    parent[adjNode] = node;
                }
            }
        }

        // If distance to a node could not be found, return an array containing -1.
        if (dist[n] == 1e9)
            return {-1};

        // Store the final path in the ‘path’ array.
        vector<int> path;
        int node = n;

        // Iterate backwards from destination to source through the parent array.
        while (parent[node] != node)
        {
            path.push_back(node);
            node = parent[node];
        }
        path.push_back(1);

        // Since the path stored is in a reverse order, we reverse the array
        // to get the final answer and then return the array.
        reverse(path.begin(), path.end());
        return path;
    }
};
#include <bits/stdc++.h>
using namespace std;


class DisjointSet {
    vector<int> rank, parent, size;
public:
    DisjointSet(int n) {
        rank.resize(n + 1, 0);
        parent.resize(n + 1);
        size.resize(n + 1);
        for (int i = 0; i <= n; i++) {
            parent[i] = i;
            size[i] = 1;
        }
    }

    int findUPar(int node) {
        if (node == parent[node])
            return node;
        return parent[node] = findUPar(parent[node]);
    }

    void unionByRank(int u, int v) {
        int ulp_u = findUPar(u);
        int ulp_v = findUPar(v);
        if (ulp_u == ulp_v) return;
        if (rank[ulp_u] < rank[ulp_v]) {
            parent[ulp_u] = ulp_v;
        }
        else if (rank[ulp_v] < rank[ulp_u]) {
            parent[ulp_v] = ulp_u;
        }
        else {
            parent[ulp_v] = ulp_u;
            rank[ulp_u]++;
        }
    }

    void unionBySize(int u, int v) {
        int ulp_u = findUPar(u);
        int ulp_v = findUPar(v);
        if (ulp_u == ulp_v) return;
        if (size[ulp_u] < size[ulp_v]) {
            parent[ulp_u] = ulp_v;
            size[ulp_v] += size[ulp_u];
        }
        else {
            parent[ulp_v] = ulp_u;
            size[ulp_u] += size[ulp_v];
        }
    }
};
class Solution
{
public:
    //Function to find sum of weights of edges of the Minimum Spanning Tree.
    int spanningTree(int V, vector<vector<int>> adj[])
    {
        // 1 - 2 wt = 5
        /// 1 - > (2, 5)
        // 2 -> (1, 5)

        // 5, 1, 2
        // 5, 2, 1
        vector<pair<int, pair<int, int>>> edges;
        for (int i = 0; i < V; i++) {
            for (auto it : adj[i]) {
                int adjNode = it[0];
                int wt = it[1];
                int node = i;

                edges.push_back({wt, {node, adjNode}});
            }
        }
        DisjointSet ds(V);
        sort(edges.begin(), edges.end());
        int mstWt = 0;
        for (auto it : edges) {
            int wt = it.first;
            int u = it.second.first;
            int v = it.second.second;

            if (ds.findUPar(u) != ds.findUPar(v)) {
                mstWt += wt;
                ds.unionBySize(u, v);
            }
        }

        return mstWt;
    }
};

class Solution
{
public:
	//Function to find sum of weights of edges of the Minimum Spanning Tree.
	int spanningTree(int V, vector<vector<int>> adj[])
	{
		priority_queue<pair<int, int>,
		               vector<pair<int, int> >, greater<pair<int, int>>> pq;

		vector<int> vis(V, 0);
		// {wt, node}
		pq.push({0, 0});
		int sum = 0;
		while (!pq.empty()) {
			auto it = pq.top();
			pq.pop();
			int node = it.second;
			int wt = it.first;

			if (vis[node] == 1) continue;
			// add it to the mst
			vis[node] = 1;
			sum += wt;
			for (auto it : adj[node]) {
				int adjNode = it[0];
				int edW = it[1];
				if (!vis[adjNode]) {
					pq.push({edW, adjNode});
				}
			}
		}
		return sum;
	}
};



