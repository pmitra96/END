# END Assignment7 Part 1

### The following was the requirement for part 1 of the assignment
    Submit the Assignment 5 as Assignment 1. To be clear,
        ONLY use datasetSentences.txt. (no augmentation required)
        Your dataset must have around 12k examples.
        Split Dataset into 70/30 Train and Test (no validation)
        Convert floating-point labels into 5 classes (0-0.2, 0.2-0.4, 0.4-0.6, 0.6-0.8, 0.8-1.0) 
        Upload to github and proceed to answer these questions asked in the S7 - Assignment Solutions, where these questions are asked:
            Share the link to your github repo (100 pts for code quality/file structure/model accuracy)
            Share the link to your readme file (200 points for proper readme file)
            Copy-paste the code related to your dataset preparation (100 pts)
            Share your training log text (you MUST have been testing for test accuracy after every epoch) (200 pts)
            Share the prediction on 10 samples picked from the test dataset. (100 pts)
            

#### The dataset is split into 70/30 train/test datasets using the following
    def get_index(perc,len_data):
      return int(perc*len_data) 
    
    len_data = df.shape[0]
    split = get_index(0.7,len_data)
    train,test = df.loc[:split-1], df.loc[split:]
    train.shape, test.shape
    
     output - ((8298, 2), (3557, 2))

#### The model used wasn't very fancy. It is an LSTM model, the following is a rough diagram -
![lstm model](https://github.com/kanchana-S/END_Assignment_5/blob/main/images/model.png)

#### The link to the notebook will be found ![here](https://github.com/pmitra96/END/blob/main/assignment7/Assignment_7_part_1.ipynb) . The training logs, and the output to 10 random samples form the test dataset can be found ![here](https://github.com/pmitra96/END/blob/main/assignment7/lstm_training_log_7_1.txt), and ![here](https://github.com/pmitra96/END/blob/main/assignment7/output_random_samples.txt) respectively, and also the notebook itself.

# Assignment 7.2 

## Quora Data set 

```
import pandas as pd

## read the data set from csv into a pandas data frame. 
quora_questions_df = pd.read_csv('quora_duplicate_questions.tsv', sep='\t', header=0)
```

We filter all the duplicate pairs of questions.
```
questions_df = quora_questions_df[quora_questions_df['is_duplicate'] == 1]
```

### Data set preperation
we define two tokenizers 

```
SRC = Field(tokenize = tokenize_en, 
            init_token = '<sos>', 
            eos_token = '<eos>', 
            lower = True)

TRG = Field(tokenize = tokenize_en, 
            init_token = '<sos>', 
            eos_token = '<eos>', 
            lower = True)
```

and we create fields entity with the following attrubutes. 

```
fields = [('src', SRC),('trg',TRG)]
```
Now each data points is represented as the field object. 

```
train_examples = [data.Example.fromlist([questions_df.question1.iloc[i], questions_df.question2.iloc[i]], fields) for i in range(questions_df.shape[0])] 
```

we now form a data set using the above examples

```
questionsDataset = data.Dataset(train_examples, fields)
```

### Test train split 
```
(train_data, test_data) = questionsDataset.split(split_ratio=[0.7, 0.3], random_state=random.seed(SEED))
```
number of samples in train_data = 104484
number of samples in test_data = 44779


### Loading the data set into iterator.
```
BATCH_SIZE = 128

train_iterator, test_iterator = BucketIterator.splits(
    (train_data, test_data),
    sort_key = lambda x: len(x.src),
    sort_within_batch=True, 
    batch_size = BATCH_SIZE, 
    device = device)
```


## QA data set


### upload your files and Mount the drive first 
```
drive.mount('/content/drive')
```

### Data parsing and loading from csv

for get questions and difficulty for each of the year's folder and aggregate all the questions 
into a single data frame.

```
import pandas as pd 
dataset = []
s08 = "/content/drive/MyDrive/Question_Answer_Dataset_v1.2/S08"
s09 = "/content/drive/MyDrive/Question_Answer_Dataset_v1.2/S09"
s10 = "/content/drive/MyDrive/Question_Answer_Dataset_v1.2/S10"

data_set_folders = [s08,s09,s10]
def prepare_dataset(folders_list):
  question_answer_pair = "question_answer_pairs.txt"
  final_data_set = []
  for folder in folders_list:
    abs_qa_path = folder + "/" + question_answer_pair
    year_data_set = generate_single_year(abs_qa_path,folder)
    print(len(year_data_set))
    final_data_set += year_data_set
  return final_data_set
    
def generate_single_year(filename,year_path):
  year_data_set = []
  with open(filename,'r',encoding="ISO-8859-1") as f:
    for line in f.readlines():
      cleaned_line = line.strip().split('\t')
      year_data_set.append(cleaned_line)
  return year_data_set

final_dataset = prepare_dataset(data_set_folders)
headers = final_dataset[0]

dict(enumerate(headers))
df = pd.DataFrame(final_dataset)[1:]
df.rename(columns=dict(enumerate(headers)),inplace=True)
df.drop(columns=["ArticleTitle","DifficultyFromQuestioner","DifficultyFromAnswerer","ArticleFile"],inplace=True)
df.head()
```

caveat:- the files are not in utf-8 encoding. 

### defining spacy tokenizers.
```
spacy_en = spacy.load('en_core_web_sm')
def tokenize_en(text):
    """
    Tokenizes English text from a string into a list of strings (tokens)
    """
    return [tok.text for tok in spacy_en.tokenizer(text)]

SRC = Field(tokenize = tokenize_en, 
            init_token = '<sos>', 
            eos_token = '<eos>', 
            lower = True)

TRG = Field(tokenize = tokenize_en, 
            init_token = '<sos>', 
            eos_token = '<eos>', 
            lower = True)
```

### Test train and val split 
```
import math as m
def get_index(train_perc,val_prec,test_prec,len_data):
  return (int(train_perc*len_data),int(val_prec*len_data),int(test_prec*test_prec))


len_data = df.shape[0]
test_split,val_split,train_split = get_index(0.8,0.1,0.1,len_data)
train,val,test = df.loc[:test_split], df.loc[test_split:test_split+val_split],df.loc[test_split+val_split:]
train.shape, test.shape, val.shape
print(train.shape)
print(test.shape)
print(val.shape)
train_list = train.values.tolist()
test_list = test.values.tolist() 
val_list = test.values.tolist()
```

### Fitting the data into torch text data.Example 
```
fields = [('Question', SRC),('Answer',TRG)]

import random
import torch, torchtext.legacy
from torchtext.legacy import data

example1 = [data.Example.fromlist([train_list[i][0], train_list[i][1]], fields) for i in range(len(train_list))]
example2 = [data.Example.fromlist([test_list[i][0], test_list[i][1] ], fields) for i in range(len(test_list))]
example3 = [data.Example.fromlist([val_list[i][0], val_list[i][1] ], fields) for i in range(len(val_list))]

train_data = data.Dataset(example1, fields)
test_data = data.Dataset(example2, fields)
valid_data = data.Dataset(example3, fields)

print(vars(train_set.examples[0]))
## {'Question': ['was', 'abraham', 'lincoln', 'the', 'sixteenth', 'president', 'of', 'the', 'united', 'states', '?'], 'Answer': ['yes']}

```

### loading data into iterator 
```
BATCH_SIZE = 64

train_iterator, valid_iterator, test_iterator = BucketIterator.splits(
    (train_data, valid_data, test_data), 
    batch_size = BATCH_SIZE, 
    sort_key= lambda x:len(x.Question),
    sort_within_batch=True,
    device = device)

```

and the rest of the code is exactly the same used in the class. 


