# Approach and Implementation
This project implements Document Similarity using the Jaccard Index with Hadoop MapReduce.

# Mapper (JaccardMapper)
Input: Each document is represented as a line with the format: DocumentID Content.
Output: For each document, the mapper tokenizes the content into unique words and emits DocumentID as the key and a comma-separated list of unique words as the value.
Objective: The mapper identifies the unique words in each document for comparison.

# Reducer (JaccardReducer)
Input: The reducer receives the document ID as the key and a list of words as the value.
Processing: The reducer compares each pair of documents:
The output is the document pair along with their Jaccard similarity score.

# Jaccard Similarity
Measures the similarity between two sets based on their intersection and union.
The score ranges from 0 (no similarity) to 1 (identical documents).

# EXECUTION:
Starting and Building the Project

```bash
docker compose up -d
```
```bash
mvn install
```

# Copying Files to the Hadoop Cluster

```bash
docker cp target/DocumentSimilarity-0.0.1-SNAPSHOT.jar resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

```bash
docker cp dataset/documents.txt resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
```
# Interacting with the Hadoop Cluster

```bash
docker exec -it resourcemanager /bin/bash
```

```bash
cd /opt/hadoop-3.2.1/share/hadoop/mapreduce/ 
```

# Setting Up HDFS and Running the Job

```bash
hadoop fs -mkdir -p /input/dataset
```

```bash
hadoop fs -put ./documents.txt /input/dataset
```

```bash
hadoop jar /opt/hadoop-3.2.1/share/hadoop/mapreduce/DocumentSimilarity-0.0.1-SNAPSHOT.jar com.example.controller.DocumentSimilarityDriver /input/dataset/documents.txt /output
```

# Retrieving the Output
```bash
hadoop fs -cat /output/*
```

```bash
hadoop fs -cat /output/output/*
```

```bash
hdfs dfs -get /output /opt/hadoop-3.2.1/share/hap/mapreduce/
```

# Exporting the Results

```bash
/opt/hadoop-3.2.1/share/hadoop/mapreduce/output/part-r-00000'
```

exit

```bash
docker cp resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/output/ output/
```
