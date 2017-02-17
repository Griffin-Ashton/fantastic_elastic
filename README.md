
# fantastic_elastic
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

## Installing Elasticsearch

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

If you wanted to rename your cluster enter the following command. 

```
./elasticsearch -Ecluster.name=my_cluster_name -Enode.name=my_node_name
```
