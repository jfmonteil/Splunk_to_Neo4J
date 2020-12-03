# Splunk to Neo4j demo with Pentaho

## Prerequisites

I have used Pentaho 8.3 EE so Splunk steps were already in. If you use CE please download Splunk step from the marketplace.

Latest steps from Neo4J here: https://github.com/knowbi/knowbi-pentaho-pdi-neo4j-output/releases/

Ktr's can be found here : https://github.com/jfmonteil/Splunk_to_Neo4J

## Demo
*tr_load_tosplunk* loads *example.csv* in Splunk. (Check the index name and filepath)

![loading to splunk](https://github.com/jfmonteil/Splunk_to_Neo4J/blob/master/tr_load_splunk.png?raw=true "Loading Splunk")

*tr_read_splunk_login* reads Splunk freshly created index and creates both Nodes (IP and Logins) and Relations in between them.

![loading logs](https://github.com/jfmonteil/Splunk_to_Neo4J/blob/master/tr_read_splunk_login.png?raw=true "transformation")

Here is the detail of Splunk input step, fill in the Connection tab accordingly, type in the good splunk query to select useful fields from the good index then press get fields button, it will populate the fields tab automatically. 
For the *src_ip* field change type form Integer to String

What we do in the transformation is regroup associated IP and user login summing a count indicator to understand the strength of the link.

Here is how to write into Neo4J without a line of Cypher.
Use the Neo4j GraphOutput
1.	Create a new Connection (that should be straightforward)
2.	Create a new Model. This will create a new graph model, see steps below for details.

 **Creating a new graph model :**

The new button should lead you to the following screen where you can import an existing JSON model. In our case we will create it through the UI. It will the be reusable from Spoon or you can export it to a JSON format.

We will now define nodes, then relationships and then we are going to map incoming transformations field to the model created.

**Nodes :**
Create as many nodes as necessary
For this step only IP and Logins are necessary, they are built with the same properties (note the Primary=Y for the id Property)
 
![enter image description here](https://github.com/jfmonteil/Splunk_to_Neo4J/blob/master/neo4j_node.png?raw=true "Defining nodes")

**Relations :**
Specify source and target nodes and optionally properties. In our case we can add a “count” property 
 ![enter image description here](https://github.com/jfmonteil/Splunk_to_Neo4J/blob/master/neo4j_realtions.png?raw=true "Relationships")

**Model check :**
You can check your schema is OK by checking the graph
 
![enter image description here](https://github.com/jfmonteil/Splunk_to_Neo4J/blob/master/neo4j_Graph.png "Graph Model summary")

**Mapping incoming fields :**
Once your model designed click OK
 
Add the following or click the “Map Fields button” for easy Mapping interface.

![enter image description here](https://github.com/jfmonteil/Splunk_to_Neo4J/blob/master/neo4j_mapping.png "mapping")

Execute the transformation.

Log into your Neo4j environment :

![enter image description here](https://github.com/jfmonteil/Splunk_to_Neo4J/blob/master/neo4jgraph.png "Relating IP and logins")

Repeat with 

 1. *tr_read_splunk_sessions*
 2. *tr_read_splunk_useragent*

You should reach the following global graph in neo4J.

![enter image description here](https://github.com/jfmonteil/Splunk_to_Neo4J/blob/master/neo4jfull.png "full_neo4J")

> Written with [StackEdit](https://stackedit.io/).
