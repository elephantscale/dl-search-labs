# Elasticsearch meets BERT lab

## Overview
We will use a pre-trained BERT model and Elasticsearch to build a search engine.

## Depends On
None

## Run time
15 to 30 mins

## Lab Goals

* Download github repository
* Download a Pre-trained BERT model
* Setting environment variables
* Launching Docker container
* Creating Elasticsearch index
* Creating documents
* Indexing document

## Summary of Lab
We will be using Elasticsearch and Bert to make a search engine.  
Elasticsearch has recently released text similarity search with vector fields.  On the other hand, you can convert text into a fixed-length vector using BERT. So once we convert documents into vectors by BERT and store them into Elasticsearch, we can search similar documents with Elasticsearch and BERT.

Example of Bertsearch
![An example of bertsearch](./docs/example.png)

## System architecture

![System architecture](./docs/architecture.png)

## STEP 0: To Instructor
* Use our ubuntu AMI, t2.large or t2.xlarge instances and Elasticsearch security group)

### STEP 1: Login to the server
 
Each student is provided their individual server and credentials

(Instructor: use our ubuntu AMI, t2.large or t2.xlarge instances and Elasticsearch security group)

### STEP 2: Download the Github repo lab is based on

* These files are needed to run lab

      wget https://github.com/Hironsan/bertsearch/archive/master.zip
      unzip master.zip
      rm master.zip

### STEP 3: Change Directory to bertsearch-master

* Need to be in this directory to run docker

      cd bertsearch-master    


### STEP 4: Download a pre-train BERT Model
* We will be using a pretrain English BERT model
* What is BERT?  
      - BERT (Bidirectional Encoder Representations from Transformers) is an open-sourced NLP pre-training model developed by researchers at Google in 2018.   
      - A direct descendant to GPT (Generalized Language Models), BERT has outperformed several models in NLP and provided top results in Question Answering (SQuAD v1.1), Natural Language Inference (MNLI), and other frameworks.

      wget https://storage.googleapis.com/bert_models/2018_10_18/cased_L-12_H-768_A-12.zip
      unzip cased_L-12_H-768_A-12.zip
      rm cased_L-12_H-768_A-12.zip  



### STEP 4: Set environment variables
* You need to set a pre-trained BERT model and Elasticsearch’s index name as environment variables. 
* These variables are used in Docker containers. 
* What are Docker containers?  
      - A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another.   
      - A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings.  
* Below is an example that specifies jobsearch as index name and ./cased_L-12_H-768_A-12 as model’s path:

      export PATH_MODEL=./cased_L-12_H-768_A-12
      export INDEX_NAME=jobsearch

    

### STEP 5: Launching Docker containers
* Now, let’s launch Docker containers by using Docker compose. 
* There are three containers to launch here: application container, BERT container, and Elasticsearch container. 
* Check docker-compose.yaml for more details. 

#Only method I could use to get docker to run without upgrading it on AWS
```
      ~/.local/bin/docker-compose up
```

You should see an output like this

    Starting bertsearch-master_elasticsearch_1 ... done
    Starting-master_bertserving_1 ... done
    
 
### Step 6:  Run docker in background
Press CTRL Z
Docker will stop and to run it in background enter the following command.
```
      bg
```

### Step 6:  Install python libraries
For scripts to work with docker install these 3 libraries to python

```
      python -m pip install elasticsearch
      python -m pip install bert_serving
      python -m pip install bert_serving-client
```
### STEP 6:  Creating Elasticsearch Index

You can use the create index API to add a new index to an Elasticsearch cluster. When creating an index, you can specify the following:
* Settings for the index
* Mappings for fields in the index
* Index aliases  

For example, if you want to create jobsearch index with title, text and text_vector fields, you can create the index by the following command:
 
```
      python example/create_index.py --index_file=example/index.json --index_name=jobsearch
```
**Caution**
The dims value of text_vector must need to match the dims of a pre-trained BERT model.

### STEP 8: Create Documents
* Once you created an index, you’re ready to index some document. The point here is to convert your document into a vector using BERT. The resulting vector is stored in the text_vector field. Let`s convert your data into a JSON document:


      python example/create_documents.py --data=example/example.csv --index_name=jobsearch   
example.csv contents
```
"Title","Description"
"Saleswoman","lorem ipsum"
"Software Developer","lorem ipsum"
"Chief Financial Officer","lorem ipsum"
"General Manager","lorem ipsum"
"Network Administrator","lorem ipsum"
After finishing script you get the following JSON document
```
documents.jsonl contents
```
{"_op_type": "index", "_index": "jobsearch", "text": "lorem ipsum", "title": "Saleswoman", "text_vector": [...]}
{"_op_type": "index", "_index": "jobsearch", "text": "lorem ipsum", "title": "Software Developer", "text_vector": [...]}
{"_op_type": "index", "_index": "jobsearch", "text": "lorem ipsum", "title": "Chief Financial Officer", "text_vector": [...]}
```

### STEP 8: Index Documents
* After converting your data into a JSON, you can adds a JSON document to the specified index and makes it searchable.


      python example/index_documents.py 


### Step 9: Open Browser
You should be able to test out the results of the search engine.   
#Not sure how to open a web browser from a linux AWS Instance.
Go to http://127.0.0.1:5000.




