
# Fantastic Elastic 

Setting up Elasticsearch on Ubuntu 14.04

### Introduction
Lately, I've been investigating Elasticsearch, and I wanted to setup a cluster on my home PC as a way of exploring some of the features. Here, I've documented my journey, and try to answer some questions a long the way. Feel free to skip to certain sections relavant, but I've tried to provide a guide starting from the ground up. This command prompts for this guide were collected from various sources, but I wanted to store them in one easy to get out location. 

[Direct Link to elastic installation guide](https://www.elastic.co/guide/en/elasticsearch/reference/current/_installation.html#_installation)

All code has been highlighted in block code. Unless noted, all code is entered in the terminal. 

## Installing Dependencies 

If you've only recently installed Ubuntu you'll need to install a few dependencies before you can install and run Elasticsearch. 

* curl
* Java - 8

### Installing cURL

The following lines will allow for you to instal both cURL and XML. 
``` 
sudo apt-get install aptitude 

sudo apt-get install libcurl4-openssl-dev 

sudo apt-get install libxml2-dev 
 ```
### Installing Java - 8

Elasticsearch requires that you have Java - 8 installed on your OS. Specifically, you need to have Java 8 - JDK.
To identify which version of java you do have, enter the following commands.

```
java -version
echo $JAVA_HOME
```
To install Java 8 - JDK

```
sudo add-apt-repository ppa:openjdk-r/ppa

sudo apt-get update

sudo apt-get install openjdk-8-jdk

sudo update-alternatives --config java

sudo update-alternatives --config javac
```
Now you need to make sure you switch to the proper java version. 

```
update-java-alternatives --list
```
If you want set a new default you can enter. (Make sure you have root permission (root means administration or superuser))

```
sudo update-java-alternatives --set /path/to/java/version
```

## Installing Elasticsearch & Setting up

Now that you've taken care of the prerequisites, you can move on to installing Elasticsearch. The process for doing this was mostly sourced from the link provided above, but I've trie to provide some additional comments based on my own experience. 

First download Elasticsearch *Note the current version of Elasticsearch and the one presented in this guide are subject to change.

We'll use curl to download the .tar file. Here, we are using to option with are curl L & O.

L - It's possible that the file we are sourcing has been moved around. If that is the case L will look for the new location and redirect our request there. 

O - write an output file named as the remote file. 
```
curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.2.1.tar.gz
```
Extract the downloaded file. 

```
tar -xvf elasticsearch-5.2.1.tar.gz
```
This creates a folder with files in your current directory. To jump into the elasticsearch folder type the following. 
```
cd elasticsearch-5.2.1/bin
```

At last, we can ENGAGE Elasticsearch. 
```
./elasticsearch
```

Here is where I ran into some differences between the output from the provided article and my machine. Mainly, no matter how hard I tried I could not clean up my terminal output to look as nice as what is presented in the guide. I also noticed a few messages were out of order when compared to the article image. No big deal, the cluster was up and running!

If you are like me and wanted to run the cluster locally, you can verify your server by opening your web browser and typeing in __127.0.0.1:9200__. If you're new to networking, this is the IP address default for connecting to a local host (the computer you're doing this all on). 

You should see this after following displayed in your browser after entering the above address. Note - cluster 'name' & 'cluster_uuid' values may be different. 

```
{
  "name" : "ThtvX1z",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "9gR-G0k6Q0yNzvS3olcU6w",
  "version" : {
    "number" : "5.2.1",
    "build_hash" : "db0d481",
    "build_date" : "2017-02-09T22:05:32.386Z",
    "build_snapshot" : false,
    "lucene_version" : "6.4.1"
  },
  "tagline" : "You Know, for Search"
}
```
To shutdown the cluster, close the terminal you used to set up the server. Once you do, revisit your local host IP address. Your webrowser should be providing with a prompt similar to 'This site can't be reached'. 

### Renaming Your Cluster

If you wanted to rename your cluster enter the following command after filling in the fields for your preferred names. This should also relaunch the cluster. Revisit your local IP address to confirm this. 

```
./elasticsearch -Ecluster.name=my_cluster_name -Enode.name=my_node_name
```
```
{
  "name" : "ChumBucket",
  "cluster_name" : "SharkTank",
  "cluster_uuid" : "9gR-G0k6Q0yNzvS3olcU6w",
  "version" : {
    "number" : "5.2.1",
    "build_hash" : "db0d481",
    "build_date" : "2017-02-09T22:05:32.386Z",
    "build_snapshot" : false,
    "lucene_version" : "6.4.1"
  },
  "tagline" : "You Know, for Search"
}
```


![alt text][Exploration]
[Exploration]: https://github.com/Griffin-Ashton/fantastic_elastic/blob/master/Shark2.jpg "Exploration"

## Cluster Exploration  

One of the major fetaures of Elasticsearch is that it comes with a REST API that allows you to interact with yoru cluster. The put it simply, a RESTFUL API is an interface that allows you to interact with your application similar to how computes and websiites talk to one another. 

Typically there are four primary commands that allow you to interact with your server. The commands are slightly different for elastic search, but the functions remaind the same.  

* GET    - retrieves the file
* PUT    - updates a file
* POST   - create (or upload) a new file
* DELETE - deletes a file

For Elasticsearch the above commands can be translated. 

* Read   = GET
* Upate  = PUT
* Create = POST
* Delete = DELETE

Additionally, with the Elasticsearch API you can perform additional operations. 

* Check in on very healthy stats and indicies on how your cluster is operating
* Perform adminstrative duters and update metadata
* Conduct advance search operations such as paging, sorting, filtering, scripting, aggregations, and many others

### Checkinng Cluster Health

The first thing we want to explore is current cluster health. To do this open up a new terminal window (as the other is still busy running your cluster). I would recommend that you interact with your cluster via CURL, but there are additional options available. 

Enter the following command, an output should be returned in your terminal window. 

```
curl -XGET 'localhost:9200/_cat/health?v&pretty'
```

As you become more familiar with the different fields, you can just return the raw information by entering the following. 

```
curl -XGET 'localhost:9200/_cat/health?'
```

Keep in mind that green always mean good. While yellow and red would be 'Not so good, Bob' and 'Tragedy' respectively. 



