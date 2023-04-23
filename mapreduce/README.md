# Map Reduce Python Example

## Install Git

```
su - root
yum install git
```

## Clone Repository
```
su - hadoop
cd /home/hadoop
git clone https://github.com/cityu-mo-course/bit010.git
```

## Start Hadoop
```
start-all.sh
```

## Upload input.txt

```
/home/hadoop/bit010/mapreduce
hdfs dfs -mkdir /wordcount
hdfs dfs -put input.ext /wordcount/
```

## Submit Job

```
hadoop jar /opt/bit010/hadoop/share/hadoop/tools/lib/hadoop-streaming-2.10.2.jar -file src/mapper.py -mapper mapper.py -file src/reducer.py -reducer reducer.py -input /wordcount/input.txt -output /wordcount/result
```

# Get Word Count Result

```
hdfs dfs -cat /wordcount/result/part-00000
```