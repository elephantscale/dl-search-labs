# Elasticsearch meets BERT lab

## Overview
We will use a pre-trained BERT model and Elasticsearch to build a search engine.
Will be split into Part 1 installing Elasticsearch and part 2 incorporating BERT  

## Depends On
None

## Run time
Unknown mins

## Lab Goals

* Install Elasticsearch and verify its operation
* Download a Pre-trained BERT model
* Setting environment variables
* Launching Docker container
* Creating Elasticsearch index
* Creating documents
* Indexing document

## STEP 0: To Instructor
* Use our ubuntu AMI, t2.large or t2.xlarge instances and Elasticsearch security group)

## Part 1 Installing Elasticsearch

### STEP 1: Login to the server
 
Each student is provided their individual server and credentials

(Instructor: use our ubuntu AMI, t2.large or t2.xlarge instances and Elasticsearch security group)

### STEP 2: Prepare the environment

* Verify that you have Java installed  
* Installation requies Java 8 and above to work with elasticsearch 

      java -version

If no java installed   
```
  sudo apt install openjdk-11-jre-headless
```

### STEP 3: Download and install Elasticsearch


      wget https://s3.amazonaws.com/elephantscale-public/downloads/elasticsearch-5.5.3.zip  #outdated but works well
      unzip elasticsearch-5.5.3.zip
      rm  elasticsearch-5.5.3.zip   
      mv elasticsearch-5.5.3 elasticsearch  

If you want to experiment with latest Elasticsearch 7
    
        wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.5.1-linux-x86_64.tar.gz
        tar -xvf elasticsearch-7.5.1-linux-x86_64.tar.gz

To get other links go here https://www.elastic.co/start 

### STEP 4: Start Elasticsearch

      cd elasticsearch/bin
      ./elasticsearch

    
_Tip_: You can run it in the background as a daemon by using the -d option

      Try this
    
      ./elasticsearch -d
    
### STEP 5: Verify install

    curl "http://localhost:9200/?pretty"
    
You should see an output like this

    {
      "name" : "gulBgf7",
      "cluster_name" : "elasticsearch",
      "cluster_uuid" : "fMqo6fq-SfqRwRCOKqrq9Q",
      "version" : {
        "number" : "6.0.0",
        "build_hash" : "8f0685b",
        "build_date" : "2017-11-10T18:41:22.859Z",
        "build_snapshot" : false,
        "lucene_version" : "7.0.1",
        "minimum_wire_compatibility_version" : "5.6.0",
        "minimum_index_compatibility_version" : "5.0.0"
      },
      "tagline" : "You Know, for Search"
    }

### Part 2

BERT can convert text into fixed-length vectors.  Once documents are converted into vectors by BERT and stored into Elasticsearch, we can search similar documents with Elasticsearch and BERT.   

Here, we use Docker to divide the whole system into three parts: application, BERT, and Elasticsearch. The purpose is to make it easier to scale each service.  
Here is the system architecture.  

## System architecture

![System architecture](./docs/architecture.png)

### 1. Download a pretrained BERT model

We will be using a pretrained English BERT model

```bash
wget https://storage.googleapis.com/bert_models/2018_10_18/cased_L-12_H-768_A-12.zip
unzip cased_L-12_H-768_A-12.zip
```
## Finish rest of Markdown need to convert upgrade current code to TF 2.0
