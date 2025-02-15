Approach and Implementation
This project implements Document Similarity using the Jaccard Index with Hadoop MapReduce.

Mapper (JaccardMapper)
Input: Each document is represented as a line with the format: DocumentID Content.
Output: For each document, the mapper tokenizes the content into unique words and emits DocumentID as the key and a comma-separated list of unique words as the value.
Objective: The mapper identifies the unique words in each document for comparison.

Reducer (JaccardReducer)
Input: The reducer receives the document ID as the key and a list of words as the value.
Processing: The reducer compares each pair of documents:
The output is the document pair along with their Jaccard similarity score.

Jaccard Similarity
Measures the similarity between two sets based on their intersection and union.
The score ranges from 0 (no similarity) to 1 (identical documents).

EXECUTION:
docker compose up -d

mvn install


docker cp target/DocumentSimilarity-0.0.1-SNAPSHOT.jar resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/


docker cp dataset/documents.txt resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/



docker exec -it resourcemanager /bin/bash


cd /opt/hadoop-3.2.1/share/hadoop/mapreduce/ 


hadoop fs -mkdir -p /input/dataset


hadoop fs -put ./documents.txt /input/dataset



hadoop jar /opt/hadoop-3.2.1/share/hadoop/mapreduce/DocumentSimilarity-0.0.1-SNAPSHOT.jar com.example.controller.DocumentSimilarityDriver /input/dataset/documents.txt /output

hadoop fs -cat /output/*

hadoop fs -cat /output/output/*


hdfs dfs -get /output /opt/hadoop-3.2.1/share/hap/mapreduce/



/opt/hadoop-3.2.1/share/hadoop/mapreduce/output/part-r-00000'


exit


docker cp resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/output/ output/
