## Prims and Kruskels - Minimum Spanning Trees

**Kruskal's algorithm** is a [greedy algorithm](http://en.wikipedia.org/wiki/Greedy_algorithm) used for finding a minimum spanning tree for a **connected**, **weighted**, and **undirected** graph. Meaning that the algorithm finds the set of edges that forms a tree including ***every vertex*** and has the ***lowest weight*** possible, without any [cycles](http://en.wikipedia.org/wiki/Cycle_(graph_theory)).

Obviously, begin with none of the edges included.

![Image](http://i58.tinypic.com/sb0i6o.png)

Then add edges one at a time, starting with the cheapest edge (if there is a tie, add either of the edges it does not matter which one you choose). Also, remember that **no cycles** are allowed in the spanning tree.


The first edge added to the tree is `E-F` since it has the least weight.

![Image2](http://i62.tinypic.com/10ded1f.png)

Continue adding edges until every vertex is connected to the tree.

The second edge, `A-E` has the next lowest weight and doesn't create a cycle, so it is added to the tree.

![Image3](http://i62.tinypic.com/s6sodw.png)

The third edge added to the tree is `C-G`, notice that the edge being added doesn't have to be connected to the tree right away.

![Image4](http://i57.tinypic.com/2rz42fd.png)

The next low-cost edge to add is `A-C`.

 ![Image5](http://i60.tinypic.com/309qznr.png)

Now either `B-C` or `D-G` can be added as the fifth edge, but I arbitrarily added `B-C` then `D-G`.

![Image6](http://i59.tinypic.com/nl33sx.png)

![Image7](http://i61.tinypic.com/fnwuty.png)

Although `C-E` has the next lowest weight, it is not added to the tree since it creates a cycle.

![Image8](http://i58.tinypic.com/2qnrsj9.png)

`B-G` also creates a cycle, so it is not added.

![Image9](http://i57.tinypic.com/f9i9as.png)

`F-G` also can't be added to the tree because it creates a cycle.

![Image10](http://i58.tinypic.com/2churtv.png)

Finally, `F-H` is added to the tree and all of the vertices are connected without any cycles.

![Image11](http://i57.tinypic.com/2087amv.png)

####Pseudo####
	KruskalMST
       Tree = Ø
       while |Tree| < n-1 do
          choose e in E of minimum cost
          E := E - {e}
          if e does not create a cycle in T then
             T := T union {e}

####Code####
	#include <cstdio>
	#include <vector>
	#include <algorithm>
	using namespace std;

	#define edge pair< int, int >
	#define MAX 1001

	// ( w (u, v) ) format
	vector< pair< int, edge > > GRAPH, MST;
	int parent[MAX], total, N, E;

	int findset(int x, int *parent)
	{
    if(x != parent[x])
        parent[x] = findset(parent[x], parent);
    return parent[x];
	}

	void kruskal()
	{
    	int i, pu, pv;
    	sort(GRAPH.begin(), GRAPH.end()); // increasing weight
    	for(i=total=0; i<E; i++)
    	{
        	pu = findset(GRAPH[i].second.first, parent);
        	pv = findset(GRAPH[i].second.second, parent);
        	if(pu != pv)
        	{
            	MST.push_back(GRAPH[i]); // add to tree
            	total += GRAPH[i].first; // add edge cost
            	parent[pu] = parent[pv]; // link
        	}
    	}
	}

	void reset()
	{
    	// reset appropriate variables here
    	// like MST.clear(), GRAPH.clear(); etc etc.
    	for(int i=1; i<=N; i++) parent[i] = i;
	}

	void print()
	{
    	`int i, sz;
    	// this is just style...
    	sz = MST.size();
    	for(i=0; i<sz; i++)
    	{
       		printf("( %d", MST[i].second.first);
        	printf(", %d )", MST[i].second.second);
        	printf(": %d\n", MST[i].first);
    	}
    	printf("Minimum cost: %d\n", total);`
	}

	int main()
	{
		int i, u, v, w;

    	scanf("%d %d", &N, &E);
    	reset();
    	for(i=0; i<E; i++)
    	{
        	scanf("%d %d %d", &u, &v, &w);
        	GRAPH.push_back(pair< int, edge >(w, edge(u, v)));
    	}
    	kruskal(); // runs kruskal and construct MST vector
    	print(); // prints MST edges and weights

    	return 0;
	}


#Works Cited

[http://zobayer.blogspot.com/2010/01/kruskals-algorithm-in-c.html](http://zobayer.blogspot.com/2010/01/kruskals-algorithm-in-c.html)
[http://en.wikipedia.org/wiki/Kruskal's_algorithm](http://en.wikipedia.org/wiki/Kruskal's_algorithm)
[http://en.wikipedia.org/wiki/Greedy_algorithm](http://en.wikipedia.org/wiki/Greedy_algorithm)
[https://www.youtube.com/watch?v=wR6JTtAmSWI](https://www.youtube.com/watch?v=wR6JTtAmSWI)


## Prim's Algorithm ##


**Prim's algorithm** is very similar to **Kruskal's Algorithm**. Both are very greedy and are used to find minimum spanning trees in weighted, connected, undirected graphs. The only real differences between the two is that Prim's algorithm can only add nodes adjacent to the current tree and any node can be added as the starting node.

![Image](http://i58.tinypic.com/sb0i6o.png)

I arbitrarily chose A as my starting point in this example, but you could start with any node. 

The blue line indicates all possible edge choices after the addition of a node to the tree and green X's indicate that an addition of that edge would create a cycle and thus it can't be added to the tree.

![](http://i58.tinypic.com/w8umc3.png)

The edge `A-E` is added since it has the lowest weight.

![](http://i61.tinypic.com/4zt9nk.png)

Then `E-F` is added.

![](http://i60.tinypic.com/2mzcuma.png)

`A-C` follows and `E-C` is no longer considered because C is already in the tree and the edge would create a cycle.

![](http://i62.tinypic.com/90verq.png)

`C-G` is the lightest edge considered so it is added and `F-G` is no longer considered.

![](http://i61.tinypic.com/dmp9nk.png)

Either `C-B` or `G-D` could be added at this point, but I chose to add `C-B` first. `A-B` and `B-G` are no longer considered.

![](http://i59.tinypic.com/15nvhht.png)

`G-D` is now added to the spanning tree. `B-D` is no longer considered. 

![](http://i59.tinypic.com/140lsfd.png)

Finally, F-H is the lightest edge remaining, so it is added to the spanning tree which is now finished.

![](http://i62.tinypic.com/4ujodi.png)

##Pseudo Code##

	Given a graph, G, with edges E of the form (v1, v2) and vertices V

	dist : array of distances from the source to each vertex
	edges: array indicating, for a given vertex, which vertex in the tree it 
	       is closest to
	i    : loop index
	F    : list of finished vertices
	U    : list or heap unfinished vertices

	/* Initialization: set every distance to INFINITY until we discover a way to
	link a vertex to the spanning tree */
	for i = 0 to |V| - 1
	    dist[i] = INFINITY
	    edge[i] = NULL
	end

	pick a vertex, s, to be the seed for the minimum spanning tree

	/* Since no edge is needed to add s to the minimum spanning tree, its distance
	from the tree is 0 */
	dist[s] = 0

	
	while(F is missing a vertex)
	   pick the vertex, v, in U with the shortest edge to the group of vertices in
	       the spanning tree add v to F
	
	   /* this loop looks through every neighbor of v and checks to see if that
	    * neighbor could reach the minimum spanning tree more cheaply through v
	    * than by linking through a previous vertex */
	   for each edge of v, (v1, v2)
	        if(length(v1, v2) < dist[v2])
	            dist[v2] = length(v1, v2)
	            edges[v2] = v1
	            possibly update U, depending on implementation
	        end if
	    end for
	end while

##Code##

	#include "ggraaf.h"
	#include <fstream>
	#include <queue>
	#include <map>
	#include <vector>
	using namespace std;

	// KnoopRelation is a class that represent a single connection
	// knoop_from = the connection starts
	// knoop_to = the connection ends
	// weight = the weight of the connection
	//
	// the operator < needs to be overloaded because we will create
	// objects of this class which will be added to a priority_queue
	// so the priority_queue needs to know on which values he need
	// to differentiate them from each other. 
	struct KnoopRelation {
		int knoop_from;
		int knoop_to;
		int weight;
		KnoopRelation(int k, int l, int g) : knoop_from(k), knoop_to(l), weight(g) {};

		bool operator<(const KnoopRelation& knoopje) const 
		{
       		return weight>knoopje.weight;   
    	}
	};

	// The prim class just calculates the minimal spanning tree
	// thats all...
	template <GraafType TYPE,typename T>
	class Prim {
		public:
			void setGraph(GewogenGraaf<TYPE, T> g);
			void minimalSpanningTree();
			bool contains(int k);
		protected:
			GewogenGraaf<TYPE, T> graph;
			priority_queue<KnoopRelation> priorqueue;
			vector<int> pad;
			vector<bool> discovered;
	};


	// Contains(int node)
	// check if a connection is not between 2 nodes that are already in the pad.
	// so only connections to nodes which aren't allready in the pad vector.
	template <GraafType TYPE,typename T>
	bool Prim<TYPE,T>::contains(int k){
		for(unsigned int i=0; i < pad.size();i++)
		if(k == pad[i])
			return true;
		else 
			return false;
	}

	// calculates minimal spanning tree with prim's algorithm
	template <GraafType TYPE,typename T>
	void Prim<TYPE,T>::minimalSpanningTree()
	{
		
		cout << "Running prim's legendary algorithm:" << endl;
		// sum of weights to calculate the total weight
		int weight = 0;
		
		// number of nodes
		unsigned int countnodes = graph.aantal_knopen();
		
		// initialize discovered
		discovered.reserve(countnodes);
		pad.reserve(countnodes);
		for(unsigned int i = 0; i < countnodes; i++)
			discovered[i] = false;
		
		// we start in 0 so this one is discovered
		int starting_point = 0;
		pad.push_back(starting_point);
		discovered[starting_point] = true;
		
		// loop for each node of the graph (so all nodes will contain the minimal spanning tree)
		// we need to add n-1 other nodes
		for(unsigned int i = 1; i < countnodes; i++)
		{
		
			// Get the connections
			// from our starting point
			map<int, int> connections = graph[starting_point];
			for(map<int, int>::iterator itr = connections.begin(); itr != connections.end(); ++itr)
			{
				// check if a connection to a new node
				if(!discovered[itr->first])
				{
					// create a new object and push it to the priority queue
					KnoopRelation rknoop(starting_point,(*itr).first,graph.gewicht((*itr).second));
					priorqueue.push(rknoop);
				}
			}

			// we cancel all bad nodes (which are already in our MOB)
			while(pad.size()!=countnodes && discovered[priorqueue.top().knoop_to])
				priorqueue.pop();
			
			// we discovered our new node that we will add
			// to our MOB
			discovered[priorqueue.top().knoop_to] = true;
			
			// get the minimal pad from the priority queue
			pad.push_back(priorqueue.top().knoop_to);
			
			// add new weight to total weight
			weight += priorqueue.top().weight;
			
			// prints out the minimal destination , from --- (weight)--> to
			cout << priorqueue.top().knoop_from << "---- (weight:"<< 
				priorqueue.top().weight <<") ---->" << 
				priorqueue.top().knoop_to << endl;
			
			// Our new starting_point
			starting_point = priorqueue.top().knoop_to;
			
		}
	
		cout << "Total weight: " << weight << endl;
	
	}

	// sets graph in the prim class
	template <GraafType TYPE,typename T>
	void Prim<TYPE,T>::setGraph(GewogenGraaf<TYPE, T> g)
	{
		graph = g;
	}


	int main() {
	
		GewogenGraaf<ONGERICHT, int> g;
		
		ifstream fileReader;
		string file = "graaf.txt";
		fileReader.open(file.c_str());
		
		// check if file exists (try opening it)
		if(!fileReader.is_open()){
			cout << "Can't open: " << file << endl;
			return 1;
		}
	
		// pass file to lees method of class ggraph.ccp
		// this method reads out the file and creates a graph structure
		try {
			g.lees(fileReader);
		} catch (GraafExceptie ge) {
			cout << ge << endl;
			return 1;
		}
	
		// create prim object and runs prim's algorithm on the weighted graph object
		Prim<ONGERICHT, int> primalgoritme;
		primalgoritme.setGraph(g);
		primalgoritme.minimalSpanningTree();
	 
		int end;
		cin >> end;
		return 0;



#Works Cited

[http://en.wikipedia.org/wiki/Prim's_algorithm](http://en.wikipedia.org/wiki/Prim's_algorithm)

[http://blog.cedric.ws/c-prims-algortihm](http://blog.cedric.ws/c-prims-algortihm)

[http://www.cprogramming.com/tutorial/computersciencetheory/mst.html](http://www.cprogramming.com/tutorial/computersciencetheory/mst.html)
