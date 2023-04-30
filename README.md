Download Link: https://assignmentchef.com/product/solved-hcp-project-5-large-scale-graph-partitioning
<br>









<h1>Graph Partitioning – Load Balancing on Parallel Architectures</h1>

New discoveries in applications often require the solution of discretized partial differential equations, nonlinear optimizations, eigenvalue computations, and management of massive data sets, such as in accelerator design, PDEconstrained optimization, or groundwater flow modeling. In such simulations, continuous infinite-dimensional mathematical models are approximated with finite-dimensional models. To obtain the required accuracy and to resolve the underlying physics, the finite-dimensional models are often large, thus requiring fast parallel computers and/or advanced algorithmic solutions. A typical application might require a sequence of numerical optimization problems to be solved. Furthermore eigenvalues and eigenvectors have to be computed, and/or nonlinear and linear systems of equations have to be solved. Porting such an application to a parallel computer would require the computational work to be equally distributed on the processors. In an adaptive simulation framework, the work distribution can also change during the computation. The work and data need to be redistributed among the compute cores to enable it to be solved quickly. Several computational tasks need to be scheduled to maximize the utilization of the processors and to reduce the idle time processors spend waiting for data or for synchronization.

A typical model in computational science and engineering is expressed using the language of continuous mathematics, such as PDEs and linear algebra, but techniques from discrete or combinatorial mathematics also play an important role in solving these models efficiently. Several discrete combinatorial problems and data structures, such as graph and hypergraph partitioning, supernodes and elimination trees, vertex and edge reordering, vertex and edge coloring, and bipartite graph matching, arise in these contexts. As an example, parallel graph partitioning tools can be used to ease the task of distributing the computational workload across the processors.

Over the past decade, parallel computers have been used with great success in many scientific simulations. While differing in their numerical methods and details of implementation, most applications successfully parallelized to date are static applications. Their data structures and memory usage do not change during the course of the computation. Their interprocessor communication patterns are predictable and nonvarying, and their processor workloads are predictable and roughly constant throughout the simulation. However, increasing use of dynamic simulation techniques is creating new challenges for developers of parallel software. For example, adaptive finite element methods refine localized regions of the mesh and adjust the order of the approximation on individual elements to obtain a desired accuracy in the numerical solution. As a result, memory must be allocated dynamically to allow creation of new elements or degrees of freedom. Communication patterns can vary as refinement creates new element neighbors. In addition, localized refinement can cause severe processor load imbalance as elemental and processor work loads change throughout a simulation. Particle simulations and N-body simulations are other examples of dynamic applications. In particle simulations, scalable parallel performance depends on a good assignment of particles to processors; grouping physically close particles within a single processor reduces interprocessor communication. Data structures and communication patterns change as particles and surfaces move. Repartitioning of the particles or surfaces is needed to maintain geometric locality of objects within processors. This scientific question can be solved with parallel graph partitioning algorithms and parallel coloring tools, e.g., based on discrete graphs and hypergraphs techniques.

High-performance graph partitioning libraries have been a very important part of scientific and engineering computing for years, and their importance continues to grow as microprocessor architectures become more complex and software libraries become better designed to integrate easily within applications. Despite the fact that there are various science

1

Figure 1: The solution of an application in computational science and engineering on a parallel computer requires scientific computing algorithms (the first row of the figure), and involves graph-theoretical research such as graph partitioning (the second row) and HPC tasks (the third row).

and engineering applications, the underlying algorithms typically have remarkable similarities, especially those algorithms that are most challenging to implement well in parallel. It is not too strong a statement to say that these graph partitioning libraries are essential to the broad success of scalable high-performance computing in computational science and engineering.

<h1>Graph Partitioning, Single-Core Optimization (100 points)</h1>

In computational science and high-performance computing, the graph partition problem is defined on data represented in the form of a graph G = (<em>V,E</em>) with <em>V </em>vertices and <em>E </em>edges, such that it is possible to partition <em>G </em>into smaller components with specific properties. For instance, a <em>k</em>-way partition divides the vertex set into <em>k </em>smaller components. In general, good solutions for the graph partitioning problem require that cuts are small and partitions have equal size. The problem arises, for instance, when assigning work to a parallel computer. In order to achieve efficiency, the workload (partition size) should be balanced evenly among the processors while communication between them (edge cut) should be kept to a minimum. Other important application domains of graph partitioning, and a detailed overview of advances in the field can be found at [?].

Recently, the graph partition problem has gained importance due to its application for clustering and detection of cliques in social, pathological, and biological networks. Since graph partitioning is an NP-hard problem, practical solutions are based on heuristics. There are two broad categories of methods, local and global. Well known local methods are the Kernighan–Lin algorithm, and Fiduccia–Mattheyses algorithms, which were the first effective 2-way cuts by local search strategies. Their major drawback is the arbitrary initial partitioning of the vertex set, which can affect the final solution quality. Global approaches rely on properties of the entire graph and not on an arbitrary initial partition. The most common example is spectral partitioning, where a partition is derived from the spectrum of the graph Laplacian matrix. In cases where the nodes of the graph are associated with a coordinate list, inertial bisection is also a frequent choice of partitioning method.

The spectral method was initially considered the standard to solve graph partitioning problems. It utilizes the spectral

<sup> </sup>0                                                          0<em>.</em>1  0             0<em>.</em>2<sup>      </sup>0<em>.</em>3      0        0             0<sup> </sup><em>n</em>

<strong>W </strong>:=    := X<em>w </em> 0 1<em>.</em>2 0 0 <em>i</em>∈<em>A,j</em>∈<em>B      j</em>=1

0<em>.</em>2    0<em>.</em>7    0<em>.</em>8        0                                                 0        0        0     1<em>.</em>7

<sup> </sup>0<em>.</em>3      −0<em>.</em>1            0     −0<em>.</em>2<sup></sup>

<strong>L </strong>:= <strong>D </strong>− <strong>W</strong><em> .</em>

Figure 2: A weighted, undirected and connected graph <em>G</em>(<em>V,E</em>), with 4 vertices and 5 edges, with its degree <strong>D</strong>, adjacency <strong>W</strong>, and graph Laplacian <strong>L </strong>matrices.

graph theorem of linear algebra, that enables the decomposition of a real symmetric matrix into eigenvalues, within an orthonormal basis of eigenvectors. For an undirected graph G(<em>V,E</em>), with vertex set <em>V </em>= {<em>v</em><sub>1</sub><em>,</em>··· <em>,v<sub>n</sub></em>} and two complementary subset <em>V</em><sub>1</sub><em>,V</em><sub>2</sub>, such that <em>V</em><sub>1 </sub>∪ <em>V</em><sub>2 </sub>= <em>V </em>and <em>V</em><sub>1 </sub>∩ <em>V</em><sub>2 </sub>= ∅, we consider an indicator vector <strong>x </strong>∈ R<em><sup>n </sup></em>such that

<em>,</em>

Some of the <em>n </em>nodes of the graph are connected via <em>m </em>edges, each of which has a positive weight associated with it, thus

(

<em>&gt; </em>0<em>, </em>if <em>v<sub>i </sub></em>connected to <em>v<sub>j</sub>, w<sub>ij </sub></em>=

= 0      otherwise.

A bisection is computed using the eigenvector associated with the second smallest eigenvalue of the graph Laplacian matrix <strong>L </strong>∈ R<em><sup>n</sup></em><sup>×<em>n</em></sup>, which encodes the degree, i.e.; the number of connected edges, of each vertex along its diagonal, and the negative weighs −<em>w<sub>ij</sub></em>. For an undirected and connected graph <em>G</em>(<em>V,E</em>), as illustrated in Figure 2, with <em>n </em>vertices and <em>m </em>edges, the graph Laplacian can be realized in terms of the adjacency matrix <strong>W </strong>∈ R<em><sup>n</sup></em><sup>×<em>n </em></sup>and the degree matrix <strong>D </strong>∈ R<em><sup>n</sup></em><sup>×<em>n </em></sup>as follows. The graph Laplacian is a symmetric positive semidefinite matrix, with its smallest eigenvalue being <em>λ</em><sup>(1) </sup>= 0, and the associated eigenvector <strong>v</strong><sup>(1) </sup>= <em>c</em><strong>1 </strong>being the constant one. The eigenvector <strong>v</strong><sup>(2) </sup>associated with the second smallest eigenvalue <em>λ</em><sup>(2) </sup>is Fiedler’s celebrated eigenvector, and is crucial for spectral graph partitioning. Each node <em>v<sub>i </sub></em>of the graph is associated with one entry in <strong>v</strong><sup>(2)</sup>. Thresholding the values of <strong>v</strong><sup>(2) </sup>around 0 results in two roughly balanced (equal sized) partitions, with minimum edgecut, while thresholding around the median value of <strong>v</strong><sup>(2) </sup>produces two strictly balanced partitions. The procedure to compute a bisection using spectral partitioning is summarized in Algorithm 1. For a much more detailed description of the method we refer to [?].

<table width="680">

 <tbody>

  <tr>

   <td width="588">Algorithm 1 Spectral bisection</td>

   <td width="92"></td>

  </tr>

  <tr>

   <td width="588">Require: G(<em>V,E</em>),Ensure: A bisection of G1 function SPECTRALBISECTION(graph G(<em>V,E</em>)) 2 Form the graph Laplacian matrix <strong>L</strong>3               Calculate the second smallest eigenvalue <em>λ</em><sup>(2) </sup>and its associated eigenvector <strong>u</strong><sup>(2)</sup>.4               Set 0 or the median of all components of <strong>u</strong><sup>(2) </sup>as threshold.5               Choose <em>V</em><sub>1 </sub>:= {<em>v<sub>i </sub></em>∈ <em>V </em>|<em>u<sub>i </sub>&lt; thres</em>}<em>,V</em><sub>2 </sub>:= {{<em>v<sub>i </sub></em>∈ <em>V </em>|<em>u<sub>i </sub></em>≥ <em>thres</em>}<em>.</em>6               return <em>V</em><sub>1</sub><em>,V</em><sub>2</sub>7               end function</td>

   <td width="92"><em>. </em>bisection of G</td>

  </tr>

 </tbody>

</table>

Inertial bisection is a very different approach, relying on the geometric layout of the graph, i.e. geometric coordinates attached to the vertices of the graph. The main idea of the method is to find a hyperlane running though the ”center of gravity of the points.” In 2 dimensions (2D) such a line <em>L </em>is chosen such that the sum of squares of the distance of the nodes to the line is minimized. It is defined by a point <em>P </em>= (<em>x,y</em>) and a vector <strong>u </strong>= (<em>u</em><sub>1</sub><em>,u</em><sub>2</sub>) with

such that <em>L </em>= {<em>P </em>+ <em>α</em><strong>u</strong>|<em>α </em>∈ R} (Figure 3). We choose

<em>,                                                                                 </em>(1)

as the center of mass lies on the line <em>L</em>. Finally, we need to compute <strong>u </strong>in order to minimize the sum of distances

<em>,                                                                                                         </em>(2)

where <em>S<sub>xx</sub>,S<sub>xy</sub>,S<sub>yy </sub></em>are the sums as defined in the previous line. The resulting matrix <em>M </em>is symmetric, thus the minimum of (2) is achieved by choosing <strong>u </strong>to be the normalized eigenvector corresponding to the smallest eigenvalue of <em>M</em>. The procedure of bisecting a graph using inertial partitioning is summarized in Algorithm 2. We refer again to [?] and references therein for a thorough overview of the inertial method.

The last task of the inertial partitioning routine (line 4), i.e partitioning nodes associated with geometric coordinates around a direction normal to the partitioning plane, is already implemented in the toolbox provided to you in function partition.m. The eigenvalue computations, both for spectral and inertial bisection should be performed with the in-Matlab function eig. Type help eig, or help eigs in your command line for more information.

Partitioning metrics

In what follows we will use the number of cut edges between partitions to determine the quality of the partitioning result. The size of an edge separator, or edgecut, which partitions the graph into a vertex subset and its complement <em>V</em><sub>1 </sub>∪ <em>V</em><sub>2 </sub>= <em>V </em>is defined as follows: cut(<em>V</em><sub>1</sub><em>,V</em><sub>2</sub>) =       <sup>X             </sup><em>w<sub>ij</sub>.          </em>(3)

<em>i</em>∈<em>V</em><sub>1</sub><em>,j</em>∈<em>V</em><sub>2</sub>

Figure 3: Illustration of inertial bisection in 2D [?].

<table width="680">

 <tbody>

  <tr>

   <td width="588">Algorithm 2 Inertial bisection.</td>

   <td width="92"></td>

  </tr>

  <tr>

   <td width="588">Require: G(<em>V,E</em>)<em>,P<sub>i </sub></em>= (<em>x<sub>i</sub>,y<sub>i</sub></em>)<em>, i </em>= 1<em>,…,n </em>the coordinates of the nodesEnsure: A bisection of G1 function INERTIALBISECTION(graph G(<em>V,E</em>)<em>,P<sub>i</sub></em>) 2 Calculate the center of mass of the points</td>

   <td width="92"><em>. </em>acc. to (1)</td>

  </tr>

  <tr>

   <td width="588">3            Compute the eigenvector associated with the smallest eigenvalue of <strong>M</strong></td>

   <td width="92"><em>. </em><strong>M </strong>acc. to (2)</td>

  </tr>

  <tr>

   <td width="588">4               Partition the nodes of the graph around line <em>L</em>.5               return <em>V</em><sub>1</sub><em>,V</em><sub>2</sub>6               end function</td>

   <td width="92"><em>. </em>bisection of G</td>

  </tr>

 </tbody>

</table>

The calculation of cut edges is implemented in the function cutsize.m of our toolbox. The cardinality of each subset (i.e the number of nodes in each partition) is given by the 2-norm of the indicator vector <strong>x</strong>

<ol>

 <li><strong>x</strong><em>. </em>(4)</li>

</ol>

Good scalability of parallel distributed memory solvers requires equally sized (balanced) cardinalities, so that the waiting time of the individual threads is minimized. For a k-way partition the load imbalance <em>b<sub>k </sub></em>is defined as the ratio of the highest partition cardinality over the average partition cardinality,

<em>,                                                                                       </em>(5)

where <em><sup>V</sup></em><sup>˜ </sup>is the average partition weight. The load imbalance, where  characterizes the deviation from obtaining an evenly balanced partitioning. The optimal value for is therefore zero, and it implies that the <em>k </em>partitions contain exactly the same number of nodes. In most applications it suffices to ensure that <em>b<sup>k</sup><sub>r </sub></em>is bounded from above by a desired threshold.

The purpose of this assignment is

<ol>

 <li>construct graphs from lists describing node connectivity</li>

 <li>to implement various graph partitioning algorithms in Matlab and to test these methods on a variety of graphs;</li>

 <li>to use a standard software package for graph partitioning (such as METIS) and to use modern computational science software engineering tools such as GitHub and mex to interface external software libraries with Matlab;</li>

 <li>to evaluate the quality of the various graph partitioning algorithm, and visualize the partitioning results.</li>

</ol>

<h1>Software tools</h1>

We will use one external partioning software tool for this HPC miniproject. METIS<a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a> [?], probably the most widely used graph partitioning software, was developed by Karypis and Kumar and is probably the most widely used graph partitioning software. It consists of a set of serial programs for partitioning graphs, partitioning finite element meshes, and producing fill reducing orderings for sparse matrices. It is based on local techniques that iteratively improve starting solutions by swapping nodes between the partitions that lie in the neighborhood of the cut. Kernighan and Lin [?] were the first to propose a local search method in this fashion. Their partition refinement strategy, which swaps pairs of vertices that result in the largest decrease in the size of the edgecut, was later accelerated by Fiduccia and Mattheyses [?]. METIS is based on this method, and subsequent improvements of it. Finally, in terms of computational efficiency, numerical experiments have demonstrated that graphs with several millions of vertices can be partitioned to 256 parts in a few seconds on current generation workstations and laptops using METIS.

<h1>The assignment</h1>

You can find the graph partitioning toolbox PartToolbox in the iCorsi webpage. This toolbox contains Matlab code for the creation of several graph and graph partitioning methods, e.g., coordinate bisection, and has routines to generate recursive multiway partitions and visualize the partitioning results.

<ol>

 <li>Install METIS 5.0.2, and the corresponding Matlab mex interface:</li>

</ol>

In order to use METIS you need the corresponding Matlab interface, since METIS is written in the C language. You can use the precompiled Matlab interface (metismex.mexmaci64) for Mac and a precompiled version for Linux (metismex.mexa64, Ubuntu, glibc 2.27). Then, all you need to do is to tell Matlab where to find the binary (see addpath command in Matlab). If you want to compile the package yourself (e.g., if you are using a different OS), read carefully the instruction on <a href="https://github.com/dgleich/metismex">https://github.com/dgleich/metismex </a>on how to install METIS 5.0.2, and how to use cmake, mex, and Matlab to build a mex interface between MATLAB and METIS.

Copy your Matlab interface metismex.mexa64 in the PartToolbox directory so that it can be used for the partitioning of meshes using METIS. Alternatively, specify the path where the interface is located (use the addpath command). Check that the interface works using the following code snippet:

<em>&gt;&gt; </em>A =          blkdiag ( ones (5) , ones ( 5 ) ) ;

<em>&gt;&gt; </em>A(1 ,10)      =      1; A(10 ,1)      =      1; A(5 ,6)      =      1; A(6 ,5)      =    1;

<em>&gt;&gt; </em>[ p1 , p2 ] = metispart ( sparse (A) ) p1 =

1            2            3            4            5

p2 =

6            7            8            9          10

<ol start="2">

 <li>Construct adjacency matrices from connectivity data [10 points]:</li>

</ol>

Your first programming task in this assignment is to construct adjacency matrices from a collection of Comma Separated Value files (.csv), describing the edge structure and the node coordinates. These files are located in datasets/countrymeshes and follow the naming convention “CountryName-NumberOfNodesFileType.csv”. The countries considered here are Great Britain, Greece, Norway, Russia, Switzerland and Vietnam. The files describing the adjacency matrices contain a list of the nodes that are connected through an edge, and the files describing the coordinates of the graph contain a list with the <em>x,y </em>coordinates of each node. The resulting graphs correspond to the continental maps of the above-mentioned countries, with the overseas territories excluded since we are interested in connected graphs. Some examples are illustrated in Figure 4.

Run the Matlab script src/readcsvgraphs.m and complete the missing sections of the code. Your goal

is to

<ul>

 <li>read the .csv files in MATLAB,</li>

 <li>construct the adjacency matrix <strong>W </strong>∈ R<em><sup>n</sup></em><sup>×<em>n </em></sup>and the node coordinate list <em>C </em>∈ R<em><sup>n</sup></em><sup>×<a href="#_ftn2" name="_ftnref2">[2]</a></sup>, and Note that <strong>W </strong>must be symmetric and sparse <sup>2</sup>.</li>

 <li>visualize the graphs of Norway and Vietnam using the function src/Visualization/gplotg.m</li>

 <li>save the sparse adjacency matrices and the corresponding coordinates in a new folder,</li>

</ul>

e.g. /datasets/countries/mat.

Figure 4: Graphs corresponding to the maps of various countries. Left: Greece with 3117 nodes and 3902 edges. Right: Switzerland with 4468 nodes and 15230 edges.

<ol start="3">

 <li>Implement various graph partitioning algorithms [30 points]:

  <ul>

   <li>Run in Matlab the script Benchbisection.m and familiarize yourself with the Matlab codes in the directory PartToolbox. An overview of all functions and scripts is offered in Contents.m.</li>

   <li>Implement spectral graph bisection based on the entries of the Fiedler eigenvector. Use the incomplete Matlab file bisectionspectral.m for your solution.</li>

   <li>Implement inertial graph bisection. For a graph with 2D coordinates, this inertial bisection constructs a line such that half the nodes are on one side of the line, and half are on the other. Use the incomplete Matlab file bisectioninertial.m for your solution.</li>

   <li>Report the bisection edgecut for all toy meshes that are loaded in the script ”Bench bisection.m.” Use Table 1 to report these results.</li>

  </ul></li>

</ol>

Table 1: Bisection results

<table width="386">

 <tbody>

  <tr>

   <td width="100">Mesh</td>

   <td width="81">Coordinate</td>

   <td width="82">Metis 5.0.2</td>

   <td width="64">Spectral</td>

   <td width="58">Inertial</td>

  </tr>

  <tr>

   <td width="100">mesh1e1</td>

   <td width="81">18</td>

   <td width="82"></td>

   <td width="64"></td>

   <td width="58"></td>

  </tr>

  <tr>

   <td width="100">mesh2e1 netz4504 dual stufe</td>

   <td width="81">37</td>

   <td width="82"></td>

   <td width="64"></td>

   <td width="58"></td>

  </tr>

 </tbody>

</table>

<ol start="4">

 <li>Recursively bisecting meshes [20 points]:</li>

</ol>

We will now partition 2D graphs emerging from structural engineering matrices of NASA, available in the SuiteSparse Matrix Collection (SSMC) [?]. A robust algorithm that allows us to create a 2<em><sup>l </sup></em>partition, with <em>l </em>being an integer, is presented in Algorithm 3. It uses the recursive function Recursion that takes as inputs the part <em>C</em><sup>0 </sup>of the graph to partition, the number of parts <em>p</em><sup>0 </sup>into which we will partition <em>C</em><sup>0</sup>, and the index (an integer) of the first part of the partition of <em>C</em><sup>0 </sup>into the final partitioning result G<em><sub>p</sub></em>. In practize, only strongly balanced bisection routines are utilized within this algorithm [?].

Algorithm 3 Recursive bisection.

Require: G(<em>V,E</em>),

Ensure: A <em>p</em>-way partition of G

<ul>

 <li>G<em><sub>p </sub></em>= {<em>C</em><sub>1</sub><em>,…,C<sub>p</sub></em>}</li>

 <li><em>p </em>= 2<em><sup>l </sup>. </em>p is a power of 2</li>

 <li>function RECURSIVEBISECTION(graph G(<em>V,E</em>), number of parts <em>p</em>)</li>

 <li>function RECURSION(<em>C</em><sup>0</sup><em>,p</em><sup>0</sup>, index)</li>

 <li>if then <em>. </em>If <em>p</em><sup>0 </sup>is even, a bisection is possible</li>

</ul>

6

7bisection(<em>C</em><sup>0</sup>)

<ul>

 <li>RECURSION<em>,</em>index)</li>

 <li>RECURSION<em>,</em>index+<em>k</em><sup>0</sup>)</li>

 <li>else <em>. </em>No more bisection possible, <em>C</em><sup>0 </sup>is in a partition of G</li>

 <li><em>C</em><em>index </em>← <em>C</em>0</li>

 <li>end if</li>

 <li>end function</li>

 <li>RECURSION(<em>C,p,</em>1)</li>

 <li>return G<em><sub>p </sub>. </em>G<em><sub>p </sub></em>is a partition of G in <em>p </em>= 2<em><sup>l </sup></em>parts</li>

 <li>end function</li>

</ul>

This algorithm is implemented in the file recbisection.m of the toolbox. Utilize this function within the script Benchrecbisection.m to recursively bisect the finite element meshes loaded within the script in 8 and 16 subgraphs. Use your inertial and spectral partitioning implementations, as well as the coordinate partitioning and the METIS bisection routine. Summarize your results in 2. Finally, visualize the results for <em>p </em>= 16 for the case ”crack”. An example for spectral recursive partitioning is illustrated in Figure 5.

<ol start="5">

 <li>Comparing recursive bisection to direct <em>k</em>-way partitioning [10 points]:</li>

</ol>

Recursive bisection is highly dependent on the decisions made during the early stages of the process, and also suffers from the lack of global information. Thus, it may result in suboptimal partitions [?]. This necessitated the development of robust methods for direct <em>k</em>-way partitioning.

Besides recursive bi-partitioning, METIS also employs a multilevel <em>k</em>-way partitioning algorithm. The graph

Table 2: Edge-cut results for recursive bi-partitioning.

<table width="354">

 <tbody>

  <tr>

   <td width="68">Case</td>

   <td width="64">Spectral</td>

   <td width="82">Metis 5.0.2</td>

   <td width="81">Coordinate</td>

   <td width="58">Inertial</td>

  </tr>

  <tr>

   <td width="68">mesh3e1airfoil1 3elt barth4 crack</td>

   <td width="64"></td>

   <td width="82"></td>

   <td width="81"></td>

   <td width="58"></td>

  </tr>

 </tbody>

</table>

Figure 5: Left: The finite element mesh ”crack” with 10240 nodes and 30380 edges. Right: 16-way recursive bisection of the mesh using Metis 5.0.2. Number of cut edges: 1290.

G = (<em>V,E</em>) is initially coarsened down to a small number of vertices, a k-way partitioning of this much smaller graph is computed, and then this partitioning is projected back towards the original finer graph by successively refining the partitioning at each intermediate level [?].

We will compare the quality of the cut resulting from the application of recursive bipartitioning and direct multiway partitioning, as implemented in Metis 5.0.2. Our test cases will be the graphs presented in Figure 6. These graphs emerge from the road networks of Luxemburg and the US, with edges representing road segments and node intersections. Graph partitioning is crucial for computing driving directions in such networks, a fundamental problem of practical importance. It can be solved in almost linear time by Dijkstra’s shortest-path algorithm, but this is not fast enough for large scale road networks. Various lightweight alternatives have been suggested [?], that use graph partitioning as a preprocessing tool to define the reduced (partitioned) topology of the network. Additionally, we will consider the map-graphs that you created in the second task of this assignment.

Use the incomplete Benchmetis.m for your implementation. Compare the cut obtained from Metis 5.0.2 after applying recursive bisection and direct multiway partitioning for the graphs in question. Consult the Metis manual, and type help metismex in your MATLAB command line to familiarize yourself with the way the Metis recursive and direct multiway partitioning functionalities should be invoked. Summarize your results

Figure 6: Left: The road network of Luxemburg with 114599 nodes and 119666 edges. Right: A segment of the US road network with 126146 nodes and 161950 edges.

in Table 3 for 16 and 32 partitions. Comment on your results. Was this behavior anticipated? Visualize the partitioning results for the graphs of i) USA, ii) Luxemburg, and iii) Russia for 32 partitions.

Table 3: Comparing the number of cut edges for recursive bisection and direct multiway partitioning in Metis 5.0.2.

<table width="563">

 <tbody>

  <tr>

   <td width="72">Partitions</td>

   <td width="84">Luxemburg</td>

   <td width="80">usroads-48</td>

   <td width="58">Greece</td>

   <td width="86">Switzerland</td>

   <td width="66">Vietnam</td>

   <td width="63">Norway</td>

   <td width="55">Russia</td>

  </tr>

  <tr>

   <td width="72">1632</td>

   <td width="84"></td>

   <td width="80"></td>

   <td width="58"></td>

   <td width="86"></td>

   <td width="66"></td>

   <td width="63"></td>

   <td width="55"></td>

  </tr>

 </tbody>

</table>

Figure 7: Partitioning the Airfoil graph based on the values of the Fiedler eigenvector. The two partitions are depicted in black and gray, while the cut edges in red respectively. The z-axis represents the value of the entries of the eigenvector.

<ol start="6">

 <li>Utilizing graph eigenvectors [30 points]:</li>

</ol>

Provide the following illustrative results. Use the incomplete script Bencheigenplot.m for your implementation.

<ol>

 <li>Plot the entries of the eigenvectors associated with the first (<em>λ</em><sub>1</sub>) and second (<em>λ</em><sub>2</sub>) smallest eigenvalues of the graph Laplacian matrix <strong>L </strong>for the graph ”airfoil1.” Comment on the visual result. Is this behavior expected?</li>

 <li>Plot the entries of the eigenvector associated with the second smallest eigenvalue <em>λ</em><sub>2 </sub>of the Graph Laplacian matrix <strong>L</strong>. Project each solution on the coordinate system space of the following graphs: mesh3e1, barth4, 3elt, crack. An example is shown in Figure 7, for the graph ”airfoil1”.</li>

</ol>

Hint: You might have to modify the functions gplotg.m and gplotpart.m to get the desired result.

<ol>

 <li>In this assignment we dealt exclusively with graphs G(<em>V,E</em>) that have coordinates associated with their nodes. This is, however, most commonly not the case when dealing with graphs, as they are in fact abstract structures, used for describing the relation <em>E </em>over a collection of entities <em>V </em>. These entities very often cannot be described in a Euclidean coordinate space. Therefore graph drawing is a tool to visualize relational information between nodes. The optimality of graph drawing is measured in terms of computation speed the ultimate usefulness of the resulting layout [?]. A successful layout should transmit the clearly the desired message, e.g the subsets of a partitioned graph. We will now see a spectral graph drawing method, which constructs the layout utilizing the eigenvectors of the graph Laplacian matrix <strong>L</strong>. Draw the graphs mesh3e1, barth4, 3elt, crack, and their spectral bi-partitioning results using the eigenvectors to supply coordinates. Locate vertex <em>i </em>at position:</li>

</ol>

<em>x<sub>i </sub></em>= (<strong>v</strong><sub>2</sub>(<em>i</em>)<em>,</em><strong>v</strong><sub>3</sub>(<em>i</em>))<em>,</em>

where <strong>v</strong><sub>2</sub><em>,</em><strong>v</strong><sub>3 </sub>are the eigenvectors associated with the 2nd and 3rd smallest eigenvalues of <strong>L</strong>. Figure 8 illustrates these 2 ways of visualizing the partitions of the ”airfoil1” graph.

Figure 8: Visualizing the bipartitioning of the graph ”airfoil1” with 4253 nodes and 12289 edges. Left: Spatial coordinates. Right: Spectral coordinates.

<h1>Additional notes and submission details</h1>

Submit the source code files (together with your used Makefile) in an archive file (tar, zip, etc.), and summarize your results and observations for all exercises by writing an extended Latex report. Use the Latex template provided on the webpage and upload the Latex summary as a PDF to <a href="https://www.icorsi.ch/login/index.php">iCorsi.</a>

<ul>

 <li>Your submission should be a gzipped tar archive, formatted like project number lastname firstname.zip or project number lastname firstname.tgz. It should contain:

  <ul>

   <li>all the source codes of your MATLAB solutions.</li>

   <li>your write-up with your name project number lastname firstname.pdf,</li>

  </ul></li>

 <li>Submit your .zip/.tgz through iCorsi.</li>

 <li>You only need MATLAB for this mini-project.</li>

</ul>

<h1>References</h1>

<ul>

 <li>E. Bichot and P. Siarry. <em>Graph Partitioning</em>. ISTE. Wiley, 2013.</li>

 <li>Aydin Buluc, Henning Meyerhenke, Ilya Safro, Peter Sanders, and Christian Schulz. <em>Recent Advances in Graph Partitioning</em>, pages 117–158. Springer International Publishing, Cham, 2016.</li>

 <li>Timothy A. Davis and Yifan Hu. The university of florida sparse matrix collection. <em>ACM Trans. Math. Softw.</em>, 38(1):1:1–1:25, December 2011.</li>

 <li>Daniel Delling and Renato F. Werneck. Faster customization of road networks. In Vincenzo Bonifaci, Camil Demetrescu, and Alberto Marchetti-Spaccamela, editors, <em>Experimental Algorithms</em>, pages 30–42, Berlin, Heidelberg, 2013. Springer Berlin Heidelberg.</li>

 <li>Elsner. Graph Partitioning: A Survey. Technical report, Technische Universitat Chemnitz, Germany, 97-27,¨ 1997.</li>

 <li>M. Fiduccia and R. M. Mattheyses. A linear-time heuristic for improving network partitions. In <em>Proceedings of the 19th Design Automation Conference</em>, DAC ’82, pages 175–181, Piscataway, NJ, USA, June 1982. IEEE</li>

</ul>

Press.

<ul>

 <li>George Karypis and Vipin Kumar. Parallel multilevel k-way partitioning scheme for irregular graphs. In <em>Proceedings of the 1996 ACM/IEEE Conference on Supercomputing</em>, Supercomputing 96, page 35es, USA, 1996. IEEE Computer Society.</li>

 <li>W. Kernighan and S. Lin. An efficient heuristic procedure for partitioning graphs. <em>The Bell System Technical Journal</em>, 49(2):291–307, Feb 1970.</li>

 <li>Koren. Drawing graphs by eigenvectors: Theory and practice. <em>Comput. Math. Appl.</em>, 49(1112):18671888, June 2005.</li>

 <li>Horst D. Simon and Shang-Hua Teng. How good is recursive bisection? <em>SIAM J. Sci. Comput.</em>, 18(5):14361445, September 1997.</li>

</ul>

<a href="#_ftnref1" name="_ftn1">[1]</a> <a href="http://glaros.dtc.umn.edu/gkhome/metis/metis/overview">http://glaros.dtc.umn.edu/gkhome/metis/metis/overview</a>

<a href="#_ftnref2" name="_ftn2">[2]</a> Check the symmetry of your resulting adjacency matrix with the function issymmetric.m. If there is an edge missing and the matrix is non-symmetric, apply the transformation <strong>W</strong><sub>sym </sub>.