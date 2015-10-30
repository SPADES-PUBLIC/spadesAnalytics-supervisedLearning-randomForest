# spadesAnalytics-supervisedLearning-randomForest
https://en.wikipedia.org/wiki/Random_forest

Description
-----------
The **SpadesAnalytics.jar** contains the Java source code and executables to run machine learning algorithms on data stored in mHealth format.  Specifically, the package **com.qmedic.spades.task.examples.spark** contains four algorithms implemented in Spark that take as input the feature extracted data (output of **com.qmedic.spades.task.examples.mapreduce.FeatureExtractor**).  

The algorithm takes five input parameters, which are passed as a string separated by commas:

1. Amazon key
2. Amazon secret key
3. Input data path (either on a local HDFS system or on Amazon S3)
4. Output data path (either on a local HDFS system or on Amazon S3)
5. Algorithm input parameters separated by a single dash

Usage
-----
Open a command prompt and type a command with the following usage pattern:

```ShellSession
java -cp SpadesAnalytics.jar [ALGORITHM] [AMAZON ACCESS KEY], [AMAZON SECRET KEY],[INPUT DATA PATH],[OUTPUT DATA PATH],[ALGORITHM PARAMETERS]
```

- **[ALGORITHM]**: (required) The desired algorithm to run on the data.  
- **[AMAZON ACCESS KEY]**: (required) The Amazon access key, needed particularly when the data is being retrieved from S3.  
- **[AMAZON SECRET KEY]**: (required) The Amazon secret key, needed particularly when the data is being retrieved from S3.
- **[INPUT DATA PATH]**: (required) The path for the input data.  This can be a local hdfs path or an S3 path.  
- **[OUTPUT DATA PATH]**: (required) The path for the output data. This can be a local hdfs path or an S3 path.  
- **[ALGORITHM PARAMETERS]**: (required) The parameters needed for the algorithm.  These are numerical parameters separated by a single dash.

Algorithm Background
--------------------
Parameters: trainfraction, numtrees, maxdepth 

Sample Input to Algorithm: 0.7-10-4

Parameter Explanations:

1. **trainfraction**: The fraction of data to be used for training.  Generally, supervised machine algorithms are implemented by building or 'training' the model on a fraction of the data and then testing the model on the remaining data.  Random Forest, Decision Tree and Logistic Regression all take this parameter as an input.  The more data you have, the smaller the trainfraction can be.
2. **numtrees**: The number of trees to use.  For the Random Forest algorithm, the number of Decision Trees used in the ensemble. For larger datasets, choose smaller numtrees to reduce computational cost.
3. **maxdepth**: The depth of the tree(s).  For tree-based algorithms like Decision Tree and Random Forest, the depth of the tree controls how specific the algorithm gets.  Deeper trees are more specific but increase the chance of overfitting.


Suggested Parameter Values:

1. trainfraction: 0.6-0.8
2. numtrees: 10-100
3. maxdepth: 4-7
 
Example Command
---------------
```ShellSession
java -cp ~/SpadesAnalytics.jar com.qmedic.spades.task.examples.spark.RandomForestHDFS yourAmazonAccessKey,yourAmazonSecretKey,hdfs://spades-data/development/InstitutionName/1/StudyName/1/MetaData-FeatureExtractor-2015-09-10-17-56-59.082/2010/07/21/*,hdfs://output-spark/output.csv,0.7-10-4
```

Output
------
Confusion matrix: a table that is used to describe the performance of a model on a set of test data for which the true values are known.  The rows represent the actual classifications and the columns represent the predicted classifications.  The more the classifications follow the diagonal of this table, the better the classifier model.

Example:
```ShellSession
HEADER_TIME_STAMP,LABELS_6
2015-10-30 11:57:11.651,basketball-dribbling,basketball-dribbling,6,basketball-short-shots,0,biking-outdoor,0,clean-room,0,play-computer-game,0,treadmill-2-mph,0
2015-10-30 11:57:11.651,basketball-short-shots,basketball-dribbling,1,basketball-short-shots,6,biking-outdoor,0,clean-room,1,play-computer-game,0,treadmill-2-mph,0
2015-10-30 11:57:11.651,biking-outdoor,basketball-dribbling,0,basketball-short-shots,0,biking-outdoor,69,clean-room,0,play-computer-game,0,treadmill-2-mph,0
2015-10-30 11:57:11.651,clean-room,basketball-dribbling,0,basketball-short-shots,0,biking-outdoor,0,clean-room,15,play-computer-game,0,treadmill-2-mph,1
2015-10-30 11:57:11.651,play-computer-game,basketball-dribbling,0,basketball-short-shots,0,biking-outdoor,0,clean-room,0,play-computer-game,20,treadmill-2-mph,0
2015-10-30 11:57:11.651,treadmill-2-mph,basketball-dribbling,0,basketball-short-shots,0,biking-outdoor,0,clean-room,0,play-computer-game,0,treadmill-2-mph,29
```
