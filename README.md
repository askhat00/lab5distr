# Word Count Streaming using MapReduce + Python

- **`wiki-input`**: Contains the dataset file `corpus.txt` (Simple English Wikipedia dump).
- **`mapper.py`**: The Mapper function.
- **`reducer.py`**: The Reducer function.

## Prerequisites

- Create and start an AWS EMR Cluster (e.g., 1 master node + N core nodes).

## Instructions

1. **Connect to the Hadoop master node using SSH.**
    ```bash
    ssh -i ~/vockey.pem hadoop@ec*-*-***-***-***.compute-*.amazonaws.com
    ```

2. **Copy the mapper and reducer scripts from the local machine to the Hadoop master node.**
    ```bash
    scp -i aws/labsuser.pem mapper.py hadoop@ec*-*-***-***-***.compute-*.amazonaws.com:/home/hadoop
    scp -i aws/labsuser.pem reducer.py hadoop@ec*-*-***-***-***.compute-*.amazonaws.com:/home/hadoop
    ```

3. **Create the input directory `wiki-input` in HDFS.**
    ```bash
    hdfs dfs -mkdir wiki-input
    ```

4. **Put the dataset file into the created directory.**
    ```bash
    hdfs dfs -put corpus.txt wiki-input
    ```

5. **Locate the required `hadoop-streaming.jar` file.**
    ```bash
    find /usr/lib/ -name *hadoop*streaming*jar
    ```
    Example output:
    ```
    /usr/lib/hadoop/hadoop-streaming.jar
    /usr/lib/hadoop-mapreduce/hadoop-streaming.jar
    ```

6. **Perform Hadoop Streaming.**  
   Output directory will be `wiki-output` when using the command below:
    ```bash
    hadoop jar /usr/lib/hadoop/hadoop-streaming.jar \
      -files mapper.py,reducer.py \
      -mapper mapper.py \
      -reducer reducer.py \
      -input wiki-input \
      -output wiki-output
    ```

7. **List the output files in HDFS.**
    ```bash
    hdfs dfs -ls wiki-output
    ```

8. **Retrieve the output files from HDFS.**
    ```bash
    hdfs dfs -get wiki-output/*
    ```

9. **Examine the content of an output file.**
    ```bash
    cat part-00000
    ```
    Example output:
    ```
    Wikipedia 12
    Simple 8
    English 15
    Article 3
    ...
    ```
