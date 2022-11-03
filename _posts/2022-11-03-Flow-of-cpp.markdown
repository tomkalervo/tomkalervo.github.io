---
layout: post
title:  "Getting the flow of c++"
date:   2022-11-03 19:15:46 +0200
categories: blog
---
If you have a bipartite graph and want to get a maximum matching - you can reduce the problem by building a flow graph and calculate its maximum flow.
Ford and Fulkerson are the inventors behind the method [Ford-Fulkerson] which calculates the maximal flow through a directed graph. When implemented using breadth-first search you get the [Edmonds-Karp] algorithm. You have to keep an adjacency list of vertices as well as edge-information for forward edges and backward edges. Using c++ I was able to effectively search, save paths and update the flow. First off we need a good way to store the data for each edge. This is done with a struct called *edgeflow* and a global vector *edge_list* that is an adjacency list (of edges).
 
{% highlight c++ %}
struct edgeflow
{
   int from, to, flow, capacity;
   struct edgeflow *reverse;
};
vector<list<edgeflow *>> edge_list;
{% endhighlight %}
 
For each edge we create an edgeflow pointer, allocate memory, set all the variables and insert the pointer into *edge_list*. For each edge we also create a backwards-edge which is linked together with the forward-edge using the reverse pointer. This edge is also added to *edge_list*.
 
{% highlight c++ %}
// for all edges (v,u), c is edge capacity of (v,u)
struct edgeflow *a = (struct edgeflow *)malloc(sizeof(struct edgeflow));
struct edgeflow *b = (struct edgeflow *)malloc(sizeof(struct edgeflow));
a->from = v;
b->from = u;
a->to = u;
b->to = v;
a->flow = 0;
b->flow = 0;
a->capacity = c;
b->capacity = 0;
a->reverse = b;
b->reverse = a;
edge_list.at(v).push_back(a);
edge_list.at(u).push_back(b);
{% endhighlight %}
 
Now we can start calculating the max flow from the node *start* to the node *sink*. While a possible path is found from *start* to *sink* we continue to increase flow along this path:
 
{% highlight c++ %}
void calculateMaxFlow()
{
   int flow = 0;
   list<edgeflow *> path;
   flow = BFS(path);
   while (!(flow == 0))
   {
       for (; !path.empty(); path.pop_front())
       {
           edgeflow *e = path.front();
           e->flow = e->flow + flow;
           e->reverse->flow = e->flow * -1;
       }
       maxflow += flow;
       flow = BFS(path);
   }
}
{% endhighlight %}
 
Here we can really utilize the pointers. The path built by *BFS* is a list with pointers to each edge. We get instant access to these objects and can update their values. Let us see how the path is found using *BFS*:
 
{% highlight c++ %}
int BFS(list<edgeflow *> &path)
{
   vector<edgeflow *> parent;
   parent.resize(vertex_total + 1, NULL);
 
   queue<int> q;
   q.push(start);
   while (!q.empty() && parent[sink] == NULL)
   {
       int v = q.front();
       q.pop();
 
       for (edgeflow *e : edge_list[v])
       {
 
           // is there capacity left  in e? If so we can use it in our path
           int possible_flow = e->capacity - e->flow;
 
           // if not visited and there exists a flow, we add edge e to the BFS-queue and mark it as visited
           if ((parent[e->to] == NULL) && (possible_flow > 0))
           {
               parent[e->to] = e;
               q.push(e->to);
           }
       }
   }
 
   if (parent[sink] == NULL)
       return 0;
   else
   {
       edgeflow *e = parent[sink];
       int min_flow = e->capacity - e->flow;
       path.push_front(e);
 
       while (e->from != start)
       {
           e = parent[e->from];
           min_flow = (e->capacity - e->flow) < min_flow ? (e->capacity - e->flow) : min_flow;
           path.push_front(e);
       }
 
       return min_flow;
   }
}
{% endhighlight %}
 
Since path is a list type it is implicitly declared as a pointer. This means that we receive its address as the argument in *BFS*. All operations done to the path inside *BFS* can be accessed outside *BFS* aswell. Looking at time efficency we see that the method of finding the maximum flow is only bound by the speed of breadth first search times the number of possible paths in the graph. 
 
[Ford-Fulkerson]: https://en.wikipedia.org/wiki/Ford?Fulkerson_algorithm
[Edmonds-Karp]: https://en.wikipedia.org/wiki/Edmonds?Karp_algorithm