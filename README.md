# Resum 

My project centers on `Lobbyists4America`, <ins>a firm operating in the legislative sector that aims to provide valuable information to its clients—the company's target audience</ins>. These clients, in turn, seek to influence legislation in the United States. Achieving this requires a strategic approach to gathering relevant information for them. To implement this strategy, they have hired me <ins>as a data scientist to analyze Congressional tweets from 2008 to 2017</ins>, shedding light on key issues, relationships, and members of Congress. Consequently, the target audience for the proposed solution is the same group that requested it: Lobbyists4America’s own clients. However, there is another group that—while not the primary focus of my project—might also be interested in the solution addressing the issues raised by Lobbyists4America. This group includes media outlets attracted to major topics discussed by Congress (especially attention-grabbing subjects like the implementation of specific laws) and the professionals within that sphere—journalists, reporters, and the outlets' respective audiences—as well as the parties upon whom Lobbyists4America’s clients sought to exert influence based on the issues highlighted by the Congressional data.


# Contents​

> Review of Questions to Answer / Hypothesis / Approach
  + Review of Questions to Answer
  + Review of Questions to Hypothesis
  + Review of Questions to Hypothesis
> Exploratory analysis 
  * Tweets Table
    * Import 
    * Data loading process
    * Import 
    * Extracting information
    * Function to convert scientific notation
    * Extracting the description
    * DuckDB Import
    * Query A
    * Query B
    * Query C
    * Additional Descriptive Statistics

* Users Table
  * Import 
  * Data loading process
  * Import 
  * Extraction
  * Extracting the description
  * DuckDB Import
  * Query A
  * Query B
  * Query C
  * Query D
  * Additional Descriptive Statistics
    
Detailed Summary

1. Tweets Table

1.1 Import 1
> Descriptive Stats



## Review of Questions to Answer / Hypothesis / Approach​

### Review of Questions to Answer

* 1- What were the main topics discussed in the Congress tweets?​

* 2- Which members played a primary role compared to the others?​

* 3- What were the key relationships between Congress and other Twitter users/accounts?

### Review of Questions to Hypothesis

* 1- The main topics are the subjects under
discussion—such as tweets regarding the
implementation of laws that typically spark
debate during the process, or controversial
issues / eye-cates issues.​

* 2- Member participation will increase in
line with the seniority of the roles assigned
within the congress.​

* 3- The key relationships within the congress
should center on the members holding the
highest-ranking positions.

### Review of Questions to Approach
 
+ First, I need to understand the type of problem I am
working on by formulating the questions I must address
during the solution process, as well as possible answers.​

+ Second, From this I will develop the practical solution;
initially I will explore and understand the organization of
the data, and as I discover this, I can refine my
relationship diagram.​

+ Third, Moving on to the analysis, I will filter, use
functions, perform joins, and code using various Python
libraries; if necessary, I will use Python to carry out more
advanced tasks involving machine learning—such as
linear regression—on the data to address a specific
question.

## Exploratory analysis

The data was extracted from Twitter in JSON format. I then performed a conversion, given that JSON is designed to facilitate data exchange between different programming languages—ultimately being converted into a format readable by the target language. My process followed this same logic: using Python and the pandas library in Visual Studio Code, I converted the received datasets into a CSV format compatible with the data tools installed on my computer—specifically SQLite, managed via SQLiteStudio. This not only made the data much easier to visualize—as I routinely use a CSV editor extension in Visual Studio Code that displays files in a table format—but also allowed me to inspect specific records to verify their accuracy and proper structure post-conversion, as well as to get a clear sense of the column organization.

The Python conversion code is this: 

```
</> Python

import pandas as pd 
with open('tweets.json', encoding='utf-8') as inputfile: 
df = pd.read_json(inputfile, lines=True) 
df.to_csv('tweets.csv', encoding='utf-8', index=False) 
```
And for the other dataset: 
```
</> Python

import pandas as pd 
with open('user.json', encoding='utf-8') as inputfile: 
df = pd.read_json(inputfile, lines=True) 
df.to_csv('user.csv', encoding='utf-8', index=False)
```

After this process, it is time to import the data into the software. In SQLiteStudio, once I have checked the columns in the CSV file, I will populate the columns in SQLiteStudio using the same names and quantity; then, I will import the data into the ta created in SQLiteStudio:

<img width="846" height="447" alt="image" src="https://github.com/user-attachments/assets/e9836a02-d500-486f-971b-e87805d2367c" />

<img width="829" height="429" alt="image" src="https://github.com/user-attachments/assets/c4b5c463-8d9f-49a6-8a89-a811c13bc527" />

To begin my exploratory analysis, I decided to find a column that established a link between the tweets table and the users table. After examining the columns, I noticed that the tweets table had a column named `user_id`, which seemed like an obvious connection point between the two; in the users table, I found the `id` column, which appeared to be the one referenced by the tweets table, so I ran a query to confirm.

```
</> SQL

SELECT tweets.user_id,
       users.id,
       users.name
FROM tweets, users;

```
```
</> SQL

Output:

      user_id   id           name
1     5558312   2915095729   Governor Bill Walker
2     5558312   33537967     Amy Klobuchar
3     5558312   1378000346   Anthony G. Brown
4     5558312   269992801    Gov. Asa Hutchinson
5     5558312   234797704    Rep. Austin Scott
6     5558312   82453460     RepBThompson
7     5558312   55677432     Bill Cassidy
8     5558312   26103389     Gov. Bill Haslam
9     5558312   15394954     U.S. Rep. Bob Latta
10    5558312   30216513     Rep. Brad Sherman
11    5558312   47747074     Brian Schatz
12    5558312   36396752     Idaho Governor
13    5558312   305620929    Dutch Ruppersberger
14    5558312   3298489662   Captain Clay Higgins
15    5558312   17976923     CathyMcMorrisRodgers
16    5558312   14984637     Chellie Pingree
17    5558312   15324851     Senator Chris Coons
18    5558312   150078976    Chris Murphy
19    5558312   18137749     Chris Van Hollen

```

```
</> SQL

SELECT tweets.user_id AS user_id_from_tweet,
       users.id AS user_id_from_users,
       users.name
FROM tweets
LEFT JOIN users
ON tweets.user_id = users.id;

```

```
</> SQL

Output:

user_id_from_tweet       user_id_from_users       name
1     5558312                  5558312                  Senator John Boozman
2     5558312                  5558312                  Senator John Boozman
3     5558312                  5558312                  Senator John Boozman
4     5558312                  5558312                  Senator John Boozman
5     5558312                  5558312                  Senator John Boozman
6     5558312                  5558312                  Senator John Boozman
7     5558312                  5558312                  Senator John Boozman
8     5558312                  5558312                  Senator John Boozman
9     5558312                  5558312                  Senator John Boozman
10    5558312                  5558312                  Senator John Boozman
11    5558312                  5558312                  Senator John Boozman
12    16056306                 16056306                 Jeff Flake
13    5558312                  5558312                  Senator John Boozman
14    5558312                  5558312                  Senator John Boozman
15    5558312                  5558312                  Senator John Boozman
16    5558312                  5558312                  Senator John Boozman
17    5558312                  5558312                  Senator John Boozman
18    5558312                  5558312                  Senator John Boozman
19    5558312                  5558312                  Senator John Boozman
```

Now that I have confirmed the hypothesis, I am using them as foreign keys.
After that, I decided to verify a piece of information requested by the company—an analysis of tweets from 2008 to 2017—and realized that the data in the `tweets` table requires no trimming or additions, given that the creation date of the first tweet is in 2008 and the last is in 2017.

```
</> SQL

Output:

created_at ⬆️       display_ text_range  entities
2008-08-04 17:28:51 [0, 74]              {'hashtags': [], 'symbols': [], 'urls': [], 'user_mentions': []}
2008-08-06 19:04:45 [0, 25]              {'hashtags': [], 'symbols': [], 'urls': [], 'user_mentions': []}
2008-08-06 20:35:36 [0, 65]              {'hashtags': [], 'symbols': [], 'urls': [], 'user_mentions': []}
2008-08-07 13:52:52 [0, 37]              {'hashtags': [], 'symbols': [], 'urls': [], 'user_mentions': []}
2008-08-07 15:12:05 [0, 90]              {'hashtags': [], 'symbols': [], 'urls': [], 'user_mentions': []}
2008-08-07 18:35:25 [0, 45]              {'hashtags': [], 'symbols': [], 'urls': [], 'user_mentions': []}
2008-08-18 14:07:35 [0, 121]             {'hashtags': [], 'symbols': [], 'urls': [], 'user_mentions': []}
2008-08-20 12:18:43 [0, 32]              {'hashtags': [], 'symbols': [], 'urls': [], 'user_mentions': []}
2008-08-21 16:24:07 [0, 84]              {'hashtags': [], 'symbols': [], 'urls': [], 'user_mentions': []}
2008-08-28 18:38:20 [0, 80]              {'hashtags': [], 'symbols': [], 'urls': [], 'user_mentions': []}
2008-09-03 02:42:32 [0, 28]              {'hashtags': [], 'symbols': [], 'urls': [], 'user_mentions': []}
2008-09-04 19:05:07 [0, 109]             {'hashtags': [{'indices':, 'text': 'rnc08'}], 'symbols': [], 'urls': [], ...
2008-09-09 16:22:47 [0, 134]             {'hashtags': [], 'symbols': [], 'urls': [], 'user_mentions': []}
2008-09-09 18:14:25 [0, 109]             {'hashtags': [], 'symbols': [], 'urls': [], 'user_mentions': []}
2008-09-09 19:15:34 [0, 6]               {'hashtags': [], 'symbols': [], 'urls': [], 'user_mentions': []}
2008-09-11 14:48:52 [0, 45]              {'hashtags': [], 'symbols': [], 'urls': [], 'user_mentions': []}
2008-09-16 20:29:08 [0, 112]             {'hashtags': [], 'symbols': [], 'urls': [], 'user_mentions': []}
2008-09-16 21:10:12 [0, 29]              {'hashtags': [], 'symbols': [], 'urls': [], 'user_mentions': []}
```
```
</> SQL

Output:

created_at ⬇️       display_text_range  entities
2017-06-06 17:16:00 [0, 119]            {'hashtags': [{'indices':, 'text': 'WrongCHOICEAct'}], 'symbols': [], 'urls': []...
2017-06-06 17:15:57 [0, 135]            {'hashtags': [{'indices':, 'text': 'VZ'}, {'indices':, 'text': 'Maduro'}]...
2017-06-06 17:15:17 [14, 153]           {'hashtags': [{'indices':, 'text': 'Questions4Betsy'}, {'indices': [141, 153]...
2017-06-06 17:15:03 [0, 140]            {'hashtags': [{'indices':, 'text': 'ComeyHearing'}], 'symbols': [], 'urls': ...
2017-06-06 17:15:01 [0, 128]            {'hashtags': [{'indices':, 'text': 'DoddFrank'}], 'symbols': [], 'urls': [], ...
2017-06-06 17:13:56 [72, 209]           {'hashtags': [{'indices':, 'text': 'Trumpcare'}, {'indices':, 'text': ...
2017-06-06 17:13:35 [14, 152]           {'hashtags': [{'indices':, 'text': 'TrumpBudget'}, {'indices':, ...
2017-06-06 17:13:17 [0, 65]             {'hashtags': [{'indices':, 'text': '603pride'}], 'media': [{'display_url': ...
2017-06-06 17:12:34 [14, 153]           {'hashtags': [{'indices':, 'text': 'Questions4Betsy'}], 'symbols': [], 'urls': []...
2017-06-06 17:11:04 [0, 140]            {'hashtags': [{'indices':, 'text': 'solar'}, {'indices':, 'text': ...
2017-06-06 17:11:04 [48, 189]           {'hashtags': [{'indices':, 'text': 'Trumpcare'}], 'media': [{'display_url': ...
2017-06-06 17:10:51 [0, 141]            {'hashtags': [{'indices':, 'text': 'PellGrants'}], 'symbols': [], 'urls': ...
2017-06-06 17:10:49 [0, 140]            {'hashtags': [{'indices':, 'text': 'WrongChoiceAct'}], 'media': ...
2017-06-06 17:10:47 [0, 134]            {'hashtags': [{'indices':, 'text': 'ProtectOurStudents'}, {'indices': [118, ...
2017-06-06 17:10:46 [0, 15]             {'hashtags': [], 'symbols': [], 'urls': [{'display_url': '://twitter.com€¦'...
2017-06-06 17:10:25 [0, 40]             {'hashtags': [], 'symbols': [], 'urls': [{'display_url': '://twitter.com€¦'...
2017-06-06 17:09:56 [14, 158]           {'hashtags': [{'indices':, 'text': 'Questions4Betsy'}], 'symbols': [], 'urls':....
2017-06-06 17:09:07 [0, 139]            {'hashtags': [], 'media': [{'display_url': '://twitter.com', ...

```

After that, I simply needed to confirm the absence of duplicate records. To do this, I selected columns in both tables where duplicates should not exist—specifically the `id` column, as it serves to uniquely identify each record. Since there were no repetitions, no rows were duplicated. I then counted the records individually using the `GROUP BY` and `COUNT` functions and verified that everything was correct, as no record count exceeded one.

```
</> SQL
SELECT id,
       Count(*) AS count_user
FROM Users
GROUP BY id
HAVING count_user > 0

```

```
</> SQL

Output:

      id           count_user
1     1009269193   1
2     1037321378   1
3     104198706    1
4     1045110018   1
5     1045853744   1
6     1048784496   1
7     1051127714   1
8     1051446626   1
9     1055685948   1
10    1055730738   1
11    1055907624   1
12    1058051748   1
13    1058256326   1
14    1058345042   1
15    1058460818   1
16    1058520120   1
17    1058717720   1
18    1058807868   1
19    1058917562   1

```

##

```
</> SQL

SELECT id,
       Count(*) AS count_user
FROM Users
GROUP BY id
HAVING count_user > 1

```

```
</> SQL

Output:

id    count_user

```

##

```
</> SQL

SELECT id,
       Count(*) AS count_tweet
FROM tweets
GROUP BY id
HAVING count_tweet > 0

```

```
</> SQL

Output:

      id                     count_tweet
1     10000021627            1
2     10000861374            1
3     10000955839873025      1
4     100010103113134080     1
5     10001321555            1
6     100041964006817792     1
7     10004390949691393      1
8     10005496345919489      1
9     10013166029246464      1
10    10016000967712768      1
11    10016592221962240      1
12    100216059579211776     1
13    100219416754659329     1
14    10022162664521728      1
15    10022562197147649      1
16    10022578844340224      1

```

##

```
</> SQL

SELECT id,
       Count(*) AS count_tweet
FROM tweets
GROUP BY id
HAVING count_tweet > 1

```

```
</> SQL

id    count_tweet

```

Since I know which columns contain no duplicate records—and indeed should not have any—I will select them as primary keys. 
After that, I examine the records in each column to classify its data type. 
Example:

```
</> SQL

Ouput:

      description
1     Representing Arkansas in the U.S. Senate. Contact Info: 202-22
2     United States Senator from the great state of Oklahoma.
3     Senior U.S. Senator, Va. Vice Chairman, Intel Committee. Previo
4     U.S. Senator. Family farmer. Lifetime resident of New Hartford,
5     Married 30 years to Cindy. Parents of four. Three dogs. Serving
6     Honored to represent the great state of Texas in the US Senate.
7     Proud Husband | Father | @Cavs @Indians @Browns Fan | Cong
8     Member of Congress from Minnesota's Fifth District. Co-Chair, @
9     Blessed to rep beautiful South Florida and the wonderful folks wh
10    Twitter feed for U.S. Senator Lindsey Graham of South Carolina.
11    Farmer, mother, Maine islander, Member of Congress, lover of r

```

```
</> SQL

description VARCHAR(45)
```

## Descriptive Stats


The primary libraries were imported to prepare the analysis environment.

### Tweets Table

#### Import 

```
</> Python

import pandas as pd
from pandasql import sqldf
import numpy as np
import ast as at
import pprint
import functools
import matplotlib
import pandasql

```

### Data loading process

The data were loaded from a JSON file using the standard read function from the pandas library, excluding a row that proved to be corrupted.

```
</> Python

import json
import pandas as pd

tweets = []

with open("tweets1.json", "r", encoding="utf-8") as f:
    for i, line in enumerate(f, start=1):
        try:
            tweets.append(json.loads(line))
        except json.JSONDecodeError:
            print(f"Linha {i} corrompida e ignorada.")

Tweets = pd.DataFrame(tweets)

print(Tweets.shape)

```

### Import 

Next, secondary supporting libraries were imported.

```
</> Python

import json
import math
import glob
import time

```

### Extracting information

A general inspection of the dataset was conducted, identifying columns, non-null values, and data types.

```
</> Python

Tweets.info()

```

```
</> Python

<class 'pandas.DataFrame'>
RangeIndex: 1243370 entries, 0 to 1243369
Data columns (total 32 columns):
 #   Column                   Non-Null Count  Dtype 
---  ------                   --------------  ----- 
 0   contributors             0 non-null      object
 1   coordinates              2734 non-null   object
 2   created_at               1243370 non-null int64 
 3   display_text_range       1243370 non-null object
 4   entities                 1243370 non-null object
 5   favorite_count           1243370 non-null int64 
 6   favorited                1243370 non-null bool  
 7   geo                      2734 non-null   object
 8   id                       1243370 non-null int64 
 9  id_str                    1243370 non-null  str    
 10 in_reply_to_screen_name   65411 non-null    str    
 11 in_reply_to_status_id     54146 non-null    float64
 12 in_reply_to_status_id_str 54146 non-null    str    
 13 in_reply_to_user_id       65411 non-null    float64
 14 in_reply_to_user_id_str   65411 non-null    str    
 15 is_quote_status           1243370 non-null  bool   
 16 lang                      1243370 non-null  str    
 17 place                     22450 non-null    object 
 18 retweet_count             1243370 non-null  int64  
 19 retweeted                 1243370 non-null  bool   
...
 30 withheld_in_countries     1 non-null        object 
 31 withheld_scope            1 non-null        str    
dtypes: bool(4), float64(3), int64(5), object(10), str(10)
memory usage: 270.4+ MB

```

--converter timestamp  olhe acima

### Obtaining descriptive statistics

Descriptive statistics were obtained, including count, mean, minimum, maximum, and quartiles.

```
</> Python

Tweets.describe()

<img width="886" height="280" alt="image" src="https://github.com/user-attachments/assets/f0e368dd-0dec-41b5-96ae-e3fab5ca4cac" />

```

-desativar notação cíentifica
pd.set_option('display.float_format', lambda x: '%.2f' % x)


### Function to convert scientific notation

### Import DuckDB

The DuckDB library was imported to enable SQL queries.

```
</> Python

import duckdb

```

### Query A

It was found that the tweets cover the period between 2008 and 2017.

<img width="782" height="560" alt="image" src="https://github.com/user-attachments/assets/2501f4c5-1f79-4809-8d8f-9244ad6fd524" />
<img width="744" height="328" alt="image" src="https://github.com/user-attachments/assets/a98fb053-c4eb-4b61-9f44-106027ee26a1" />
<img width="712" height="520" alt="image" src="https://github.com/user-attachments/assets/041a419c-95d3-4870-9b23-79a5961ed4f3" />

### Query B

The profiles most retweeted by Congress were identified—revealing relationships between parliamentarians and other accounts—alongside descriptions of the profiles.

<img width="886" height="320" alt="image" src="https://github.com/user-attachments/assets/58da40fb-0687-485e-a487-274deadf26cb" />
<img width="886" height="315" alt="image" src="https://github.com/user-attachments/assets/3632e59e-5cfd-4328-b15d-8607c45f98c2" />
<img width="552" height="324" alt="image" src="https://github.com/user-attachments/assets/5c3db795-347c-4d9f-8923-db1b6ac56f21" />
<img width="878" height="349" alt="image" src="https://github.com/user-attachments/assets/7adabe42-1cad-40d7-a1da-69852d528813" />
<img width="855" height="330" alt="image" src="https://github.com/user-attachments/assets/33995b5a-041f-4dfd-b6a5-c4cf4ca01a2f" />
<img width="846" height="327" alt="image" src="https://github.com/user-attachments/assets/2f45d159-d04b-4bc2-85cf-5e6f358ad83f" />
<img width="869" height="345" alt="image" src="https://github.com/user-attachments/assets/7bddcc99-c1ec-4e34-8df7-8c2361d8ef2e" />
<img width="850" height="329" alt="image" src="https://github.com/user-attachments/assets/af985132-9226-4410-aa41-5733e1929b6e" />
<img width="852" height="179" alt="image" src="https://github.com/user-attachments/assets/5931cc92-6d27-4f97-a81b-a7d20fcc03b1" />

### Query C

The users responsible for the highest number of tweets on behalf of Congress and their profile descriptions were identified.
Corrigir
<img width="822" height="336" alt="image" src="https://github.com/user-attachments/assets/041e5f99-5d09-4814-8abd-325133c9b5d6" />
<img width="502" height="454" alt="image" src="https://github.com/user-attachments/assets/e85cd2e4-ee88-44f6-be3f-6e8cafc8a7f0" />
<img width="803" height="292" alt="image" src="https://github.com/user-attachments/assets/9293e6d9-95c0-433c-a280-a81573e8af00" />
<img width="841" height="348" alt="image" src="https://github.com/user-attachments/assets/8e2b283c-313f-4c51-9a0f-73eeee642f67" />
<img width="806" height="320" alt="image" src="https://github.com/user-attachments/assets/61f80b65-c89c-4aa6-bd38-6dfddac5bf30" />
<img width="824" height="324" alt="image" src="https://github.com/user-attachments/assets/5364569d-e87d-4b05-ad74-1e8cc8f537a2" />
<img width="833" height="304" alt="image" src="https://github.com/user-attachments/assets/427c758b-db84-4a0c-aa1a-0e1c58ae6ca5" />

### Additional Descriptive Statistics

Code to extract (from the columns where the necessary calculations could be performed) the mean, mode, population/sample standard deviation, range, and variance.

##### Mode

```
</> Python

mode = Tweets.mode()
print(mode)
```
##### Meam

```
</> Python

 #Convert all possible columns to numeric
Mean = Tweets.apply(pd.to_numeric, errors="coerce")

 #Calculate the mean of each column
means = Mean.mean()

print(means)

```

```
</> Python

Output:

contributors                 NaN
coordinates                  NaN
created_at          1.433799e+09
display_text_range           NaN
entities                     NaN
favorite_count      2.008517e+02
favorited          1.447678e-05
geo                          NaN
id                 6.096596e+17
id_str             6.096596e+17
in_reply_to_screen_name    1.436980e+04
in_reply_to_status_id    6.624784e+17
in_reply_to_status_id_str 6.624784e+17
in_reply_to_user_id     3.055319e+16
in_reply_to_user_id_str 3.055319e+16
is_quote_status      4.609328e-02
lang                         NaN
place                        NaN
retweet_count      1.900628e+02
retweeted          0.000000e+00
screen_name                  NaN
source                       NaN
text               4.538053e+01
truncated         0.000000e+00

```

in progress...


```
</> Python

```
