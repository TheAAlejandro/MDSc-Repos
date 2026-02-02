#Word Frequency Pipeline on Hadoop 

This project implements a **Word Frequency Analysis** pipeline using Hadoop MapReduce. 

## Running the Word Frequency pipeline

The file titled frequency_reducer.py is located under /home/aldie/hadoop-3.4.2/lab2 folder. This file contains the the code which returns the relative frequency of the word with respect to the total number of words found in the document. 

To run the frequency pipeline on Hadoop, we first input the document in the input folder under hdfs. We do this by running the following command:

```bash
bin/hdfs dfs -mkdir -p /user/<your-username>/lab2/input
bin/hdfs dfs -put <file-to-upload> /user/<your-usernam>/lab2/input/

Next, we save the files required to map the words and compute for its frequency under mapper.py and frequency.py, respectively, to a corresponding folder. We can save these files under lab2.

```bash
mv mapper.py lab2/
mv frequency_reducer.py lab2/

Next, we run the following command to perform frequency analysis on your chosen file:

```bash
bin/hadoop jar share/hadoop/tools/lib/hadoop-streaming-*.jar \
  -input /user/<your-username>/lab2/input/bible.txt \
  -output /user/<your-username>/lab2/frequency_output \
  -mapper /home/<your-username>/hadoop-3.4.2/lab2/mapper.py \
  -reducer /home/<your-username>/hadoop-3.4.2/lab2 /reducer.py
```

We can view the result of the word frequencies by running the command

```bash
bin/hdfs dfs -cat /user/<your-username>/lab2/frequency_output/part-00000 
```

## Differences from word count 
The word frequency pipeline is different with the word count pipeline. The former computes for the relative frequency of a certain word with respect to the entire document (or the entire corpus of words) while the word count only provides the absolute count or instances at which the word appears in the document. 

Word frequencies return percentage values signifying how much the document contains that specific word, and that this count is normalized across all the unique words found in the document. For instance, if the word "the" appears in the document 50 times in a 1000-word document, the word count is "50" while the frequency of it is "0.05".

## Sample output format

The output format for the frequency pipeline is the following:

```bash
word\tfrequency
```

where \t is a tab space between the word and its frequency in 2 decimal places. 

A sample output is provided for the first 5 words for the Aesop file which can be downloaded by using the command

```bash
wget https://www.gutenberg.org/cache/epub/21/pg21.txt

Sample output:
#21\t0.00
$1\t0.00
$5,000\t0.00
&c\t0.00
***\t0.01
```

## HDFS Paths Used
The hdfs paths used in the pipeline are as follows:

Purpose	| HDFS Path
Input folder | /user/<your-username>/lab2/input/
Output folder | /user/<your-username>/lab2/frequency_output/

## Sort by Frequency
We can, in fact, sort the results of the word frequency pipeline just by running the following command.

```bash
bin/hdfs dfs -cat /user/<your-username>/lab2/frequency_output/part-00000 | sort -t$'\t' -k2 -nr | head -20
```

The command utilizes the following syntax:
- sort : sorts the lines of text (in our case, the result of the frequency)
- -t$'\t' : specifies the delimeter used for splitting the text into two columns (where the delimeter is a tab space or \t)
- k2 : specifies that we sort by the second column or the frequency
- n : performs numeric sort
- r : returns the result in a reverse order, which makes the highest frequency comes first
