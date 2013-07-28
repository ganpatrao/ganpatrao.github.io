--- 
layout: post
title: "GraphML With JUNG: Loading From GraphML"
categories: 
- Code
- Java
- JUNG
tags: 
- graphml
- jung
- loading graphs from xml
status: publish
type: post
published: true
meta: 
  _wp_old_slug: ""
  _wpas_skip_twitter: "1"
  _wpas_skip_fb: "1"
---
This is a follow up to my <a title="GraphML With JUNG: Saving Out" href="http://halfclosed.wordpress.com/2010/12/04/graphml-with-jung-saving/" target="_blank">earlier post</a> on saving to GraphML using the JUNG library. It would make more sense if you browsed through my <a title="GraphML With JUNG: Saving Out" href="http://halfclosed.wordpress.com/2010/12/04/graphml-with-jung-saving/" target="_blank">earlier post</a> before reading this one.
[sourcecode lang="java" light="true"]BufferedReader fileReader = new BufferedReader(
                            new FileReader(filename));[/sourcecode]
You first need to read from your file. This snippet creates a standard Java <code>BufferedReader,</code> which in turn is fed from a <code>FileReader </code>that reads from the file specified by the <code>String filename</code>.

<!--more-->The problem is, the <code>GraphMLReader2</code> just a smart text parser, for it to do anything useful, you need to specify <strong>Transformers</strong> for the vertices, edges, hyperedges and the graph itself. We pump these transformers into the function reading the graph so it understands what exactly to do after it reads these different components in from the file.
[sourcecode lang="java"] /* Create the Graph Transformer */
 Transformer&lt;GraphMetadata, Graph&lt;MyVertex, MyEdge&gt;&gt;
 graphTransformer = new Transformer&lt;GraphMetadata,
                           Graph&lt;MyVertex, MyEdge&gt;&gt;() {

   public Graph&lt;MyVertex, MyEdge&gt;
       transform(GraphMetadata metadata) {
         if (metadata.getEdgeDefault().equals(
         metadata.getEdgeDefault().DIRECTED)) {
             return new
             DirectedSparseGraph&lt;MyVertex, MyEdge&gt;();
         } else {
             return new
             UndirectedSparseGraph&lt;MyVertex, MyEdge&gt;();
         }
       }
 };
[/sourcecode]
This transformer is used to parse the <code></code> tag in my GraphML file and create a new <code>Graph</code> object by transforming that <code>String</code>. (it would really help if you had your GraphML file open at this point to understand what the transformers are doing). This tag specifies the beginning of your graph, and whether the graph is directed or undirected. The function <code>metadata.getEdgeDefault() </code>in this case returns <strong>"undirected" </strong>which is equal to the library-defined constant <code>metadata.getEdgeDefault().DIRECTED. </code>So this transformer checks whether the graph in the file is directed and returns the required <code>Graph </code>object.
[sourcecode lang="java"]/* Create the Vertex Transformer */
Transformer&lt;NodeMetadata, MyVertex&gt; vertexTransformer
= new Transformer&lt;NodeMetadata, MyVertex&gt;() {
    public MyVertex transform(NodeMetadata metadata) {
        MyVertex v =
            MyVertexFactory.getInstance().create();
        v.setX(Double.parseDouble(
                           metadata.getProperty(&quot;x&quot;)));
        v.setY(Double.parseDouble(
                           metadata.getProperty(&quot;y&quot;)));
        return v;
    }
};[/sourcecode]
The vertex transformer is slightly more involved. I'll list out the various parts:
<ul>
	<li><code>MyVertexFactory: </code>This is my custom <code>Factory</code> that generates objects of my custom <code>MyVertex</code> class.</li>
	<li><code>MyVertexFactory.getInstance()</code>: This returns an object of the MyVertexFactory class that I can now use to access it's methods.</li>
	<li><code>v.setX(Double d)/v.setY(Double d)</code>: These are custom functions I've defined for my <code>MyVertex</code> class that set the <code>private int x</code> and <code>private int y</code> properties of the current vertex to the specified value.</li>
	<li><code>metadata.getProperty("x")</code>/<code>metadata.getProperty("y")</code>: This gets the value of the <strong>"x"</strong> and <strong>"y" </strong>properties that I've stored in my GraphML file (we used the <code>addVertexData </code>function in the previous post to save these out).</li>
</ul>
The vertex transformer parses the vertex data from the file and reads in its "x" and "y" coordinates, and then sets the <strong>x</strong> and <strong>y</strong> properties on the newly created <code>MyVertex</code> object.
[sourcecode lang="java"]/* Create the Edge Transformer */
 Transformer&lt;EdgeMetadata, MyEdge&gt; edgeTransformer =
 new Transformer&lt;EdgeMetadata, MyEdge&gt;() {
     public MyEdge transform(EdgeMetadata metadata) {
         MyEdge e = MyEdgeFactory.getInstance().create();
         return e;
     }
 };[/sourcecode]
Similar to the previous transformer, this edge transformer reads in the edge data from the file and creates a new edge. The <code>MyEdgeFactory</code> and <code>getInstance()</code> are analogous to those just discussed for the <code>MyVertex</code> class.
[sourcecode lang="java"]/* Create the Hyperedge Transformer */
Transformer&lt;HyperEdgeMetadata, MyEdge&gt; hyperEdgeTransformer
= new Transformer&lt;HyperEdgeMetadata, MyEdge&gt;() {
     public MyEdge transform(HyperEdgeMetadata metadata) {
         MyEdge e = MyEdgeFactory.getInstance().create();
         return e;
     }
};[/sourcecode]
This is pretty much the same code as my edge transformer! My graphs didn't have hyper-edges, but the GraphMLReader2 definition <strong>requires</strong> that you specify a hyperedge transformer too. So let's just get it done with. Finally, you create your graph reader object:
[sourcecode lang="java"]/* Create the graphMLReader2 */
GraphMLReader2&lt;Graph&lt;MyVertex, MyEdge&gt;, MyVertex, MyEdge&gt;
graphReader = new
GraphMLReader2&lt;Graph&lt;MyVertex, MyEdge&gt;, MyVertex, MyEdge&gt;
      (fileReader, graphTransformer, vertexTransformer,
       edgeTransformer, hyperEdgeTransformer);[/sourcecode]
It's a pretty long statement to create one object, so make sure you've got all your angle brackets in place.
[sourcecode lang="java"]try {
    /* Get the new graph object from the GraphML file */
    Graph g = graphReader.readGraph();
} catch (GraphIOException ex) {}[/sourcecode]
Now you have your Graph object! Assuming you've worked with simple JUNG Graph objects enough to want to save them out, you should be able to take it from here. There is a small catch though; the graph right now will be displayed without any regard to the <strong>x</strong> and <strong>y</strong> properties associated with its vertex objects. So you'll need to specify a <code>StaticLayout</code> with a custom transformer, that reads in the x and y properties of each vertex and places them accordingly on your canvas. You do that like this:
[sourcecode lang="java"]StaticLayout&lt;MyVerex, MyEdge&gt; layout =
 new StaticLayout(g, new Transformer&lt;MyVertex, Point2D&gt;() {
   public Point2D transform(MyVertex v) {
       Point2D p = new Point2D.Double(v.getX(), v.getY());
       return p;
   }               
 });[/sourcecode]
This transformer is required to convert a vertex object to a <code>Point2D</code> object that the layout needs to position its vertices. It creates a new Point2D object by using the custom <code>getX()</code> and <code>getY()</code> methods that return the x and y properties of the vertex.
