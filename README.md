# Resum 

My project centers on `Lobbyists4America`, <ins>a firm operating in the legislative sector that aims to provide valuable information to its clients—the company's target audience</ins>. These clients, in turn, seek to influence legislation in the United States. Achieving this requires a strategic approach to gathering relevant information for them. To implement this strategy, they have hired me <ins>as a data scientist to analyze Congressional tweets from 2008 to 2017</ins>, shedding light on key issues, relationships, and members of Congress. Consequently, the target audience for the proposed solution is the same group that requested it: Lobbyists4America’s own clients. However, there is another group that—while not the primary focus of my project—might also be interested in the solution addressing the issues raised by Lobbyists4America. This group includes media outlets attracted to major topics discussed by Congress (especially attention-grabbing subjects like the implementation of specific laws) and the professionals within that sphere—journalists, reporters, and the outlets' respective audiences—as well as the parties upon whom Lobbyists4America’s clients sought to exert influence based on the issues highlighted by the Congressional data.

# Contents​

> Review of Questions to Answer / Hypothesis / Approach
  + Review of Questions to Answer
  + Review of Questions to Hypothesis
  + Review of Questions to Hypothesis
    </>br   
> Exploratory analysis

>  Descriptive Stats
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

### Transforming Data

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

### Loading Data

After this process, it is time to import the data into the software. In SQLiteStudio, once I have checked the columns in the CSV file, I will populate the columns in SQLiteStudio using the same names and quantity; then, I will import the data into the ta created in SQLiteStudio:

## 

<img width="846" height="447" alt="image" src="https://github.com/user-attachments/assets/e9836a02-d500-486f-971b-e87805d2367c" />

<img width="829" height="429" alt="image" src="https://github.com/user-attachments/assets/c4b5c463-8d9f-49a6-8a89-a811c13bc527" />

##

### Foreign key

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

### Criation date

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

### Primary key

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

### Data type

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

### Entity Relationship Diagram (ERD)

Now that the veracity of the data, types, and relationships has been verified, I created an Entity-Relationship Diagram (ERD) based on my findings, using MySQL Workbench 6.0 software.

<img width="573" height="1057" alt="image" src="https://github.com/user-attachments/assets/df41cb68-8ff8-4bed-b6f2-04e1a664dfda" />

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

Descriptive statistics were obtained, including count, mean, minimum, maximum, and quartiles. Function to convert scientific notation and  timestamp.

```
</> Python

import pandas as pd

# 1. Change the general display to floats without scientific notation
pd.set_option('display.float_format', lambda x: '%.2f' % x)

# 2. Convert the 'created_at' column from Unix Timestamp to Real Date
Tweets['created_at'] = pd.to_datetime(Tweets['created_at'], unit='s')

# 3. Execute the describe function including the formatted dates
Tweets.describe(datetime_is_numeric=True)

```
```
</> Python

Output:

| Statics           | created_at          | favorite_count |                      id |   in_reply_to_status_id | in_reply_to_user_id | retweet_count |            user_id |        quoted_status_id |
| :---------------- | :------------------ | -------------: | ----------------------: | ----------------------: | ------------------: | ------------: | -----------------: | ----------------------: |
| **Count**         | 1,243,370           |      1,243,370 |               1,243,370 |                  54,146 |              65,411 |     1,243,370 |          1,243,370 |                  56,418 |
| **Mean**          | 2015-06-11 21:10:00 |         200.85 | 609,659,600,000,000,000 | 662,478,400,000,000,000 |  30,553,190,000,000 |        190.06 | 13,974,050,000,000 | 775,152,900,000,000,000 |
| **Std**           | 52,263,240          |       3,545.41 | 214,092,500,000,000,000 | 210,676,800,000,000,000 | 152,843,500,000,000 |      9,944.39 | 10,533,830,000,000 |   7,897,866,000,000,000 |
| **Min**           | 2008-08-04 17:28:51 |           0.00 |              87,741,860 |           1,059,864,000 |                  20 |          0.00 |          5,558,312 |              90,957,760 |
| **25%**           | 2014-06-14 02:40:00 |           0.00 | 476,795,200,000,000,000 | 552,497,700,000,000,000 |          30,380,230 |          1.00 |         33,750,800 | 725,045,100,000,000,000 |
| **50% (median)** | 2015-11-04 16:50:00 |           2.00 | 662,381,800,000,000,000 | 739,907,400,000,000,000 |         204,905,600 |          4.00 |        234,022,300 | 794,551,000,000,000,000 |
| **75%**           | 2016-10-02 02:30:00 |           8.00 | 781,241,600,000,000,000 | 831,621,200,000,000,000 |         622,731,300 |         10.00 |         99,315,300 | 839,266,200,000,000,000 |
| **Max**           | 2017-06-06 17:16:00 |     984,832.00 | 872,140,000,000,000,000 | 872,139,400,000,000,000 | 866,745,300,000,000 |  3,637,896.00 | 85,471,510,000,000 | 872,138,300,000,000,000 |

```

### Import DuckDB

The DuckDB library was imported to enable SQL queries.

```
</> Python

import duckdb

```

### Query A

It was found that the tweets cover the period between 2008 and 2017.

```
</> Python
query = "SELECT make_timestamp(CAST(created_at AS BIGINT) * 1000000) AS data_formatada FROM Tweets ORDER BY created_at DESC"
result = duckdb.sql(query).df()
print(result)
```

```
</> Python
|        Nº |  created_at         |
| --------: | ------------------- |
|         1 | 2017-06-06 17:16:00 |
|         2 | 2017-06-06 17:15:57 |
|         3 | 2017-06-06 17:15:17 |
|         4 | 2017-06-06 17:15:03 |
|         5 | 2017-06-06 17:15:01 |
|         ⋮ | ⋮                    |
| 1.243.366 | 2008-08-11 20:38:45 |
| 1.243.367 | 2008-08-11 20:32:52 |
| 1.243.368 | 2008-08-11 18:48:56 |
| 1.243.369 | 2008-08-11 18:41:25 |
| 1.243.370 | 2008-08-11 11:02:11 |
```

```
</> Python
query = "SELECT make_timestamp(CAST(created_at AS BIGINT) * 1000000) AS data_formatada FROM Tweets ORDER BY created_at ASC"
result = duckdb.sql(query).df()
print(result) 
```

```
</> Python
|        Nº | created_at          |
| --------: | ------------------- |
|         1 | 2008-08-04 17:28:51 |
|         2 | 2008-08-06 18:41:25 |
|         3 | 2008-08-06 18:48:56 |
|         4 | 2008-08-07 11:52:52 |
|         5 | 2008-08-07 13:05:25 |
|         ⋮ | ⋮                    |
| 1.243.366 | 2017-06-06 17:15:01 |
| 1.243.367 | 2017-06-06 17:15:03 |
| 1.243.368 | 2017-06-06 17:15:17 |
| 1.243.369 | 2017-06-06 17:15:57 |
| 1.243.370 | 2017-06-06 17:16:00 |
```

### Query B

The profiles most retweeted by Congress were identified—revealing relationships between parliamentarians and other accounts—alongside descriptions of the profiles.

```
</> Python

query = "SELECT in_reply_to_screen_name, COUNT (in_reply_to_screen_name) as retweets_name FROM Tweets Group By in_reply_to_screen_name Order By retweets_name DESC"
result = duckdb.sql(query).df()

print(result)

```

```
</> Python

Output:

|     Nº | in_reply_to_screen_name | Retweets |
| -----: | ----------------------- | -------: |
|      1 | SenatorDurbin           |     1309 |
|      2 | SenGillibrand           |     1011 |
|      3 | SenFeinstein            |      798 |
|      4 | RepMcGovern             |      753 |
|      5 | SenBobCasey             |      736 |
|      ⋮ | ⋮                       |        ⋮ |
| 26.363 | apusateri               |        1 |
| 26.364 | Astro_Pam               |        1 |
| 26.365 | AVDIT6                  |        1 |
| 26.366 | ruchiikatalwar          |        1 |
| 26.367 | NaN                     |        0 |

```

```
</> SQL
SELECT 
    t.in_reply_to_screen_name,
    u.name,
    u.description,
    COUNT(in_reply_to_screen_name) AS retweets_name
FROM Tweets t
JOIN Users u
    ON t.in_reply_to_screen_name = u.screen_name
GROUP BY 
    t.in_reply_to_screen_name,
    u.name,
    u.description
ORDER BY retweets_name DESC

```

```
</> SQL

Output:

| Nº | in_reply_to_screen_name | Name                 | Description                                                            | Retweets |
| -: | ----------------------- | -------------------- | ---------------------------------------------------------------------- | -------: |
|  1 | SenatorDurbin           | Senator Dick Durbin  | Serving the people of Illinois. Senate Democratic Whip...              |     1309 |
|  2 | SenGillibrand           | Kirsten Gillibrand   | Theo and Henry's mom, U.S. Senator from New York...                    |     1011 |
|  3 | SenFeinstein            | Sen Dianne Feinstein | United States Senator from California. On Facebook...                  |      798 |
|  4 | RepMcGovern             | Jim McGovern         | Proudly representing the people of the 2nd District...                 |      753 |
|  5 | SenBobCasey             | Senator Bob Casey    | Official Twitter profile for U.S. Senator Bob Casey...                 |      736 |
|  6 | SenJeffMerkley          | Senator Jeff Merkley | U.S. Senator from the State of Oregon. Instagram...                    |      612 |
|  7 | GovMalloyOffice         | Governor Dan Malloy  | Official account of the Office of Connecticut Governor Dan Malloy...   |      545 |
|  8 | PattyMurray             | Senator Patty Murray | Official account of U.S. Senator Patty Murray...                       |      531 |
|  9 | RepAdamSchiff           | Adam Schiff          | Representing California's 28th Congressional District...               |      513 |
| 10 | RonWyden                | Ron Wyden            | U.S. Senator from Oregon. Ranking Member of @SenateFinance...          |      439 |
| 11 | SenatorTomUdall         | Tom Udall            | United States Senator from New Mexico.                                 |      381 |
| 12 | ChrisMurphyCT           | Chris Murphy         | U.S. Senator, Connecticut.                                             |      377 |
| 13 | RepDonBeyer             | Rep. Don Beyer       | Representing Virginia's 8th District in Congress...                    |      368 |
| 14 | SenFranken              | Sen. Al Franken      | Official account of U.S. Senator Al Franken...                         |      346 |
| 15 | RepScottPeters          | Rep. Scott Peters    | Serving #CA52. @EnergyCommerce, @VetAffairsDem...                      |      337 |
| 16 | SenMarkey               | Ed Markey            | Official account for Senator Edward J. Markey...                       |      285 |
| 17 | RepCicilline            | David Cicilline      | Tweets from the press office of Congressman David Cicilline...         |      259 |
| 18 | NYGovCuomo              | Andrew Cuomo         | Father, fisherman, motorcycle enthusiast, 56th Governor of New York... |      250 |
| 19 | timkaine                | Senator Tim Kaine    | U.S. Senator from Virginia. Husband and father...                      |      233 |
| 20 | RepRaskin               | Rep. Jamie Raskin    | Proudly serving #MD08 in the U.S. House of Representatives...          |      232 |
| 21 | SenatorBaldwin          | Sen. Tammy Baldwin   | Official Twitter account of United States Senator Tammy Baldwin...     |      229 |
| 22 | RepLipinski             | Rep. Daniel Lipinski | Proudly representing the people of Illinois' 3rd District...           |      227 |
| 23 | SenatorHeitkamp         | Sen. Heidi Heitkamp  | Official Twitter account for Heidi Heitkamp...                         |      226 |
| 24 | SenatorHassan           | Sen. Maggie Hassan   | Official Twitter account of U.S. Senator Maggie Hassan...              |      211 |
| 25 | WhipHoyer               | Steny Hoyer          | Democratic Whip of the U.S. House of Representatives...                |      204 |
| 26 | SenDuckworth            | Tammy Duckworth      | Official Twitter account for Tammy Duckworth...                        |      201 |
| 27 | SenSanders              | Bernie Sanders       | Longest-serving Independent member of Congress...                      |      201 |
| 28 | SusanWBrooks            | Susan W. Brooks      | Serving Indiana's 5th Congressional District...                        |      199 |
| 29 | realDonaldTrump         | Donald J. Trump      | 45th President of the USA.                                             |      197 |
| 30 | RepDelBene              | Rep. Suzan DelBene   | U.S. Congresswoman representing Washington's 1st District...           |      195 |
| 31 | GinaRaimondo            | Gina Raimondo        | Governor of Rhode Island dedicated to moving Rhode Island forward...   |      180 |
| 32 | POTUS                   | President Trump      | 45th President of the United States of America...                      |      178 |
| 33 | RepDonaldPayne          | Donald Payne Jr.     | Proudly serving New Jersey's 10th District.                            |      178 |
| 34 | RepMarthaRoby           | Rep. Martha Roby     | Congressional Twitter account for U.S. Representative Martha Roby...   |      174 |
| 35 | ChrisCoons              | Senator Chris Coons  | Father, husband, U.S. Senator from Delaware...                         |      171 |
| 36 | FrankPallone            | Rep. Frank Pallone   | Official Twitter account of Congressman Frank Pallone...               |      168 |
| 37 | RepEsty                 | Elizabeth Esty       | Mom, community advocate, and U.S. Representative...                    |      165 |
| 38 | GovernorDeal            | Governor Nathan Deal | Governor Nathan Deal is the 82nd Governor of Georgia.                                                |      164 |
| 39 | RepGaramendi            | John Garamendi       | Congressman for California's 3rd Congressional District...                                           |      158 |
| 40 | RepKathleenRice         | Kathleen Rice        | U.S. Representative for #NY04 (Long Island). Former Nassau County DA...                              |      153 |
| 41 | GovernorTomWolf         | Governor Tom Wolf    | Official account for Pennsylvania Governor Tom Wolf. Updates...                                      |      150 |
| 42 | SenWarren               | Elizabeth Warren     | Official Twitter account of Senator Elizabeth Warren of Massachusetts...                             |      147 |
| 43 | SenatorCarper           | Senator Tom Carper   | U.S. Senator for Delaware | Ranking Member of @EPWDems.                                              |      146 |
| 44 | RepBarbaraLee           | Rep. Barbara Lee     | Progressive Democrat proudly representing California's East Bay...                                   |      143 |
| 45 | OregonGovBrown          | Governor Kate Brown  | Official Twitter presence of Kate Brown, Oregon's 38th Governor...                                   |      129 |
| 46 | senorrinhatch           | Senator Hatch Office | U.S. Senator from Utah. Tweets are by Sen. Hatch's staff...                                          |      126 |
| 47 | chelliepingree          | Chellie Pingree      | Farmer, mother, Maine islander, Member of Congress...                                                |      124 |
| 48 | WVGovernor              | Governor Jim Justice | Jim Justice is the 36th Governor of the State of West Virginia.                                      |      120 |
| 49 | repjimcooper            | Jim Cooper           | Official feed of U.S. Rep. Jim Cooper. Blue Dog Democrat...                                          |      115 |
| 50 | SenSherrodBrown         | Sherrod Brown        | Office of United States Senator Sherrod Brown...                                                     |      106 |
| 51 | RepSarbanes             | Rep. John Sarbanes   | Representing Maryland's 3rd District. Chair of the Democratic Policy and Communications Committee... |      101 |
| 52 | RepSinema               | Kyrsten Sinema       | Official Twitter feed of U.S. Congresswoman Kyrsten Sinema...                                        |      100 |
| 53 | ChrisVanHollen          | Chris Van Hollen     | U.S. Senator for Maryland.                                                                           |       97 |
| 54 | RepDWSweets             | D. Wasserman Schultz | U.S. Congresswoman proudly representing the people of Florida...                                     |       97 |
| 55 | SenWhitehouse           | Sheldon Whitehouse   | U.S. Senator from Rhode Island, the Ocean State.                                                     |       97 |
| 56 | SenJohnMcCain           | John McCain          | U.S. Senator John McCain (R-AZ), Chairman of the Senate Armed Services Committee... |       94 |
| 57 | repjoecrowley           | Rep. Joe Crowley     | Proudly representing New York's 14th Congressional District...                      |       92 |
| 58 | GOPLeader               | Kevin McCarthy       | I serve as Majority Leader and Representative...                                    |       87 |
| 59 | SenBennetCO             | Michael F. Bennet    | U.S. Senator for Colorado.                                                          |       87 |
| 60 | SenKamalaHarris         | Kamala Harris        | Official Twitter account of U.S. Senator Kamala Harris...                           |       87 |
| 61 | SenBlumenthal           | Richard Blumenthal   | Connecticut's U.S. Senator.                                                         |       86 |
| 62 | NitaLowey               | Nita Lowey           | Proudly representing New York's 17th District...                                    |       85 |
| 63 | SenSchumer              | Chuck Schumer        | Official account of Senator Chuck Schumer – New York.                               |       85 |
| 64 | RepStefanik             | Rep. Elise Stefanik  | Official Twitter account for Congresswoman Elise Stefanik...                        |       84 |
| 65 | SenAngusKing            | Senator Angus King   | News from the office of Independent U.S. Senator Angus King...                      |       84 |
| 66 | RepRoKhanna             | Rep. Ro Khanna       | Representing #CA17 in Silicon Valley. Working to expand opportunity...              |       83 |
| 67 | justinamash             | Justin Amash         | I defend #liberty and explain every vote...                                         |       83 |
| 68 | RepSchneider            | Rep. Brad Schneider  | Proud to represent the people of Illinois' 10th District...                         |       80 |
| 69 | RepTimRyan              | Congressman Tim Ryan | Proud Husband | Father | @Cavs @Indians @Browns...                                  |       80 |
| 70 | SenBobCorker            | Senator Bob Corker   | Official news and updates from United States Senator Bob Corker...                  |       77 |
| 71 | SenStabenow             | Sen. Debbie Stabenow | Representing the people of the Great State of Michigan...                           |       77 |
| 72 | repblumenauer           | Earl Blumenauer      | Congressman.                                                                        |       77 |
| 73 | RepJerryNadler          | (((Rep. Nadler)))    | Representing parts of Manhattan and Brooklyn...                                     |       76 |
| 74 | PramilaJayapal          | Pramila Jayapal      | Congressmember, WA-07. Advocate for justice...                              |       72 |
| 75 | RepMcEachin             | Rep. Donald McEachin | Father, husband & Progressive Democrat in Congress...                       |       72 |
| 76 | RepWalorski             | Jackie Walorski      | Representing Indiana's Second Congressional District.                       |       72 |
| 77 | SenatorCardin           | Senator Ben Cardin   | U.S. Senator Ben Cardin of Maryland. Ranking Member...                      |       71 |
| 78 | louiseslaughter         | Louise Slaughter     | Honored to serve the people of Rochester and Monroe County...               |       69 |
| 79 | MartinHeinrich          | Martin Heinrich      | United States Senator from New Mexico.                                      |       68 |
| 80 | RepMarkTakano           | Mark Takano          | Proudly representing California's 41st Congressional District...            |       68 |
| 81 | RepMarcyKaptur          | Marcy Kaptur         | Office of Congresswoman Marcy Kaptur. Longest-serving woman in the House... |       64 |
| 82 | RepKihuen               | Rep. Ruben J. Kihuen | U.S. Representative proudly serving Nevada's 4th District...                |       63 |
| 83 | NC_Governor             | Governor Roy Cooper  | Official account of the 75th Governor of North Carolina.                    |       62 |
| 84 | TulsiGabbard            | Rep. Tulsi Gabbard   | Aloha – Official account of Rep. Tulsi Gabbard...                           |       61 |
| 85 | keithellison            | Rep. Keith Ellison   | Member of Congress from Minnesota's Fifth District...                       |       60 |
| 86 | SenatorShaheen          | Sen. Jeanne Shaheen  | Proud Granite Stater serving New Hampshire in the U.S. Senate...            |       59 |
| 87 | maziehirono             | Senator Mazie Hirono | U.S. Senator Mazie K. Hirono – Proudly serving Hawaii...                    |       57 |
| 88 | RepDanKildee            | Rep. Dan Kildee      | Honored to represent Michigan's Fifth Congressional District...             |       56 |
| 89 | NancyPelosi             | Nancy Pelosi         | Democratic Leader, focused on strengthening America...                      |       55 |
| 90 | RepBradWenstrup         | Brad Wenstrup        | U.S. Representative for Ohio's 2nd Congressional District...                |       55 |
| 91 | RepTedDeutch            | Rep. Ted Deutch      | Ranking Member of the Ethics Committee. Member of...                        |       54 |
|  92 | DonaldNorcross          | Donald Norcross        | Representing New Jersey's 1st Congressional District...                            |       53 |
|  93 | govsambrownback         | Sam Brownback          | Official Twitter page for Kansas Governor Sam Brownback.                           |       53 |
|  94 | RepPaulTonko            | Paul Tonko             | U.S. Representative for New York's 20th Congressional District...                  |       52 |
|  95 | SenatorBurr             | Richard Burr           | U.S. Senator from North Carolina, Chairman of the Senate Intelligence Committee... |       52 |
|  96 | RepShimkus              | John Shimkus           | I represent the 15th Congressional District of Illinois...                         |       51 |
|  97 | RodneyDavis             | U.S. Rep. Rodney Davis | Proudly representing the 13th Congressional District of Illinois...                |       50 |
|  98 | GerryConnolly           | Gerry Connolly         | Proudly representing Virginia's 11th Congressional District.                       |       49 |
|  99 | RepFredUpton            | Fred Upton             | Husband, Dad, and proud to represent Michigan's 6th Congressional District...      |       49 |
| 100 | SenRonJohnson           | Senator Ron Johnson    | Chairman of the Homeland Security and Governmental Affairs Committee...            |       49 |


[100 rows x 4 columns]



```

### Query C

The users responsible for the highest number of tweets on behalf of Congress and their profile descriptions were identified.

```
</> Python
query = "SELECT user_id, COUNT (user_id) as tweets_user_id FROM Tweets Group By user_id Order By tweets_user_id DESC"
result = duckdb.sql(query).df()

print(result)
```
```
</> Python
               user_id  tweets_user_id
0            2962868158            3258
1             247334603            3252
2              18023868            3250
3            3141213592            3250
4              14845376            3249
..                  ...             ...
540           115676070              80
541           510196665              37
542  854715071116849157              27
543  818536152588238849              16
544            20334348               4

[545 rows x 2 columns]

```
```
</> Python
query = """
SELECT 
    t.user_id,
    u.name,
    u.description,
    COUNT(*) AS tweets_user_id
FROM Tweets t
JOIN Users u
    ON t.user_id = u.id
GROUP BY 
    t.user_id,
    u.name,
    u.description
ORDER BY tweets_user_id DESC
LIMIT 30
"""

resultado = duckdb.sql(query).df()
print(resultado)
```

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

Output (Expalined):

+-----------------------------+------------------------------------------------------+
| Column Name                 | Converted Value / Meaning                            |
+-----------------------------+------------------------------------------------------+
| contributors                | NaN (Null / Empty)                                   |
| coordinates                 | NaN (Null / Empty)                                   |
| created_at                  | 2015-06-11 21:10:00 (Average Date)                   |
| display_text_range          | NaN (Null / Empty)                                   |
| entities                    | NaN (Null / Empty)                                   |
| favorite_count              | 200.85 (Average likes per tweet)                     |
| favorited                   | 0.00 (Exactly 0%)                                    |
| geo                         | NaN (Null / Empty)                                   |
| id                          | 609,659,600,000,000,000                              |
| id_str                      | 609,659,600,000,000,000                              |
| in_reply_to_screen_name     | 14,369.80                                            |
| in_reply_to_status_id       | 662,478,400,000,000,000                              |
| in_reply_to_status_id_str   | 662,478,400,000,000,000                              |
| in_reply_to_user_id         | 30,553,190,000,000,000                               |
| in_reply_to_user_id_str     | 30,553,190,000,000,000                               |
| is_quote_status             | 0.0461 (≈ 4.61% of all tweets)                        |
| lang                        | NaN (Null / Empty)                                   |
| place                       | NaN (Null / Empty)                                   |
| retweet_count               | 190.06 (Average retweets per tweet)                  |
| retweeted                   | 0.00 (Exactly 0%)                                    |
| screen_name                 | NaN (Null / Empty)                                   |
| source                      | NaN (Null / Empty)                                   |
| text                        | 45.38 (Average text length in characters)            |
| truncated                   | 0.00 (Exactly 0%)                                    |
| user_id                     | 13,974,050,000,000,000                               |
| withheld_copyright          | 1.00 (True / 100%)                                   |
| withheld_in_countries       | NaN (Null / Empty)                                   |
| withheld_scope              | NaN (Null / Empty)                                   |
+-----------------------------+------------------------------------------------------+
```

```
</> Python

deviation = Tweets.apply(pd.to_numeric, errors="coerce")

population standard deviation = deviation.mode(numeric_only=True)

print(population standard deviation)

```

```
</> Python

Output ( Explained ):

| **Column Name**       | **Mode (Most Frequent Value)** | **Meaning**                                                                |
| --------------------- | -----------------------------: | -------------------------------------------------------------------------- |
| contributors          |                            NaN | No mode (all values are null)                                              |
| coordinates           |                            NaN | No mode (all values are null)                                              |
| created_at            |                   1.423237e+09 | Unix timestamp corresponding to the most frequent date                     |
| display_text_range    |                            NaN | No mode (all values are null)                                              |
| entities              |                            NaN | No mode (all values are null)                                              |
| favorite_count        |                            0.0 | Most tweets received 0 likes                                               |
| favorited             |                          False | Tweets were generally not favorited by the authenticated user              |
| geo                   |                            NaN | No geographic information available                                        |
| id                    |                      877418565 | Smallest/first unique value returned as mode (IDs are unique)              |
| id_str                |                      877418565 | String representation of the tweet ID                                      |
| text                  |                       3.141593 | Most frequent numeric value after conversion (non-numeric text became NaN) |
| truncated             |                          False | Tweets were generally not truncated                                        |
| user_id               |                   2.962868e+09 | Most frequent user ID after numeric conversion                             |
| possibly_sensitive    |                            0.0 | Tweets were generally not marked as sensitive                              |
| extended_entities     |                            NaN | No mode (all values are null)                                              |
| quoted_status_id      |                   8.592532e+17 | Most frequent quoted tweet ID after numeric conversion                     |
| quoted_status_id_str  |                   8.592532e+17 | String representation of the quoted tweet ID                               |
| withheld_copyright    |                            1.0 | Copyright restriction flag set to True                                     |
| withheld_in_countries |                            NaN | No country restrictions recorded                                           |
| withheld_scope        |                            NaN | No withholding scope specified                                             |

```

```
</> Python

variations = Tweets.var(numeric_only=True)
print(variations)

```

```
</> Python

numbers = Tweets.select_dtypes(include=["int64", "float64"])

amplitude = numbers.max() - numbers.min()

print(amplitude)

```


```
</> Python

Output ( Explained ):

| **Column Name**       | **Range (Converted Value)** | **Meaning**                                                   |
| --------------------- | --------------------------: | ------------------------------------------------------------- |
| created_at            |                 278,898,400 | Range of tweet creation dates (Unix timestamp)                |
| favorite_count        |                     984,832 | Difference between the maximum and minimum number of likes    |
| id                    |     872,140,000,000,000,000 | Difference between the largest and smallest tweet IDs         |
| in_reply_to_status_id |     872,139,400,000,000,000 | Difference between the largest and smallest replied tweet IDs |
| in_reply_to_user_id   |     866,745,300,000,000,000 | Difference between the largest and smallest replied user IDs  |
| retweet_count         |                   3,637,896 | Difference between the maximum and minimum number of retweets |
| user_id               |     854,715,100,000,000,000 | Difference between the largest and smallest user IDs          |
| quoted_status_id      |     872,138,300,000,000,000 | Difference between the largest and smallest quoted tweet IDs  |

```

```
</> Python

numbers = Tweets.select_dtypes(include=[np.number])

sample standard deviation = numbers.std()

print(sample standard deviation)

```

```
</> Python

Output ( Expalined ):

| **Column Name**       | **Standard Deviation (Converted Value)** | **Meaning**                                                 |
| --------------------- | ---------------------------------------: | ----------------------------------------------------------- |
| created_at            |                               52,263,240 | Standard deviation of tweet creation dates (Unix timestamp) |
| favorite_count        |                                3,545.405 | Standard deviation of the number of likes per tweet         |
| id                    |                  214,092,500,000,000,000 | Standard deviation of tweet IDs                             |
| in_reply_to_status_id |                  210,676,800,000,000,000 | Standard deviation of replied tweet IDs                     |
| in_reply_to_user_id   |                  152,843,500,000,000,000 | Standard deviation of replied user IDs                      |
| retweet_count         |                                9,944.392 | Standard deviation of the number of retweets                |
| user_id               |                  105,338,300,000,000,000 | Standard deviation of user IDs                              |
| quoted_status_id      |                   78,978,660,000,000,000 | Standard deviation of quoted tweet IDs                      |


```

## User Table

### Import

The main libraries were imported.

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

The JSON file was loaded using pandas (JSON reading function).

```
</> Python

Users = pd.read_json('users1.json', lines = True)

```

### Import

Auxiliary libraries were imported to complement the analysis.

```
</> Python

import json
import math
import glob
import time

```

### Extracting information

Inspection of columns, data types, and non-null values.
```
</> Python

Users.info()

| **#** | **Column Name**                    | **Non-Null Count** | **Data Type** |
| ----: | ---------------------------------- | -----------------: | :------------ |
|     0 | contributors_enabled               |                548 | bool          |
|     1 | created_at                         |                548 | object        |
|     2 | default_profile                    |                548 | bool          |
|     3 | default_profile_image              |                548 | bool          |
|     4 | description                        |                545 | str           |
|     5 | entities                           |                545 | object        |
|     6 | favourites_count                   |                548 | int64         |
|     7 | follow_request_sent                |                548 | bool          |
|     8 | followers_count                    |                548 | int64         |
|     9 | following                          |                548 | bool          |
|    10 | friends_count                      |                548 | int64         |
|    11 | geo_enabled                        |                548 | bool          |
|    12 | has_extended_profile               |                548 | bool          |
|    13 | id                                 |                548 | int64         |
|    14 | id_str                             |                548 | int64         |
|    15 | is_translation_enabled             |                548 | bool          |
|    16 | is_translator                      |                548 | bool          |
|    17 | lang                               |                548 | str           |
|    18 | listed_count                       |                548 | int64         |
|    19 | location                           |                548 | str           |
|    20 | name                               |                548 | str           |
|    21 | notifications                      |                548 | bool          |
|    22 | profile_background_color           |                548 | str           |
|    23 | profile_background_image_url       |                512 | str           |
|    24 | profile_background_image_url_https |                512 | str           |
|    25 | profile_background_tile            |                548 | bool          |
|    26 | profile_banner_url                 |                513 | str           |
|    27 | profile_image_url                  |                548 | str           |
|    28 | profile_image_url_https            |                548 | str           |
|    29 | profile_link_color                 |                548 | str           |
|    30 | profile_sidebar_border_color       |                548 | str           |
|    31 | profile_sidebar_fill_color         |                548 | str           |
|    32 | profile_text_color                 |                548 | str           |
|    33 | profile_use_background_image       |                548 | bool          |
|    34 | protected                          |                548 | bool          |
|    35 | screen_name                        |                548 | str           |
|    36 | statuses_count                     |                548 | int64         |
|    37 | time_zone                          |                503 | str           |
|    38 | translator_type                    |                548 | str           |
|    39 | url                                |                513 | str           |
|    40 | utc_offset                         |                503 | float64       |
|    41 | verified                           |                548 | bool          |

| **Attribute**                      | **Value** |
| ---------------------------------- | --------- |
| Total Columns                      | 42        |
| Total Rows                         | 548       |
| Boolean Columns                    | 14        |
| Integer Columns (`int64`)          | 7         |
| Floating-Point Columns (`float64`) | 1         |
| Object Columns                     | 2         |
| String Columns (`str`)             | 18        |
| Memory Usage                       | 127.5 KB  |

```

### Generating the description

Generation of descriptive statistics for the dataset.

```
</> Python

Users.describe()

```

```
</> Python

Outpu:

| **Index** | **contributors_enabled** | **created_at (Converted)** | **default_profile** | **...** | **url**                      | **utc_offset** | **verified** | ...
| --------: | :----------------------- | :------------------------- | :------------------ | :------ | :--------------------------- | -------------: | :----------- |
|         0 | False                    | **2014-12-01 03:47:17**    | True                | ...     | [http://t.co](http://t.co)   |            NaN | True         |
|         1 | False                    | **2009-04-20 16:19:36**    | False               | ...     | [http://t.co](http://t.co)   |       -18000.0 | True         |
|         2 | False                    | **2013-04-25 00:26:33**    | False               | ...     | [https://t.co](https://t.co) |       -14400.0 | True         |
|         3 | False                    | **2011-03-22 01:52:54**    | False               | ...     | [https://t.co](https://t.co) |       -18000.0 | True         |
|         4 | False                    | **2011-01-06 21:21:46**    | False               | ...     | [http://t.co](http://t.co)   |            NaN | True         |
|         ⋮ | ⋮                        | ⋮                          | ⋮                   | ⋮       | ⋮                            |              ⋮ | ⋮            |
|       543 | False                    | **2008-09-12 14:50:07**    | False               | ...     | [http://t.co](http://t.co)   |       -14400.0 | True         |
|       544 | False                    | **2016-06-14 17:35:24**    | True                | ...     | [https://t.co](https://t.co) |       -25200.0 | True         |
|       545 | False                    | **2009-02-26 15:33:01**    | False               | ...     | [https://t.co](https://t.co) |       -14400.0 | True         |
|       546 | False                    | **2012-06-13 13:28:01**    | False               | ...     | [http://t.co](http://t.co)   |       -14400.0 | True         |
|       547 | False                    | **2016-10-23 18:23:37**    | True                | ...     | NaN                          |            NaN | False        |

```

### Import DuckDB

Import DuckDB to execute queries.

```
</> Python

import duckdb

```

### Query A

Analyze profile creation data, noting the temporal alignment with the period of the tweets (demonstrating that the profiles were created at the end of the conference participation).

```
</> Python

query = "SELECT created_at FROM Users Order By created_at DESC"
result = duckdb.sql(query).df()

print(result)
```
```
</> Python

Output:

            created_at
0    25/05/2010 16:02:26
1    07/04/2015 19:36:11
2    23/10/2016 18:23:37
3    19/04/2017 12:08:47
4    02/02/2017 19:10:40
..                   ...
543  29/12/2007 18:32:03
544  26/11/2007 03:57:02
545  12/07/2007 02:43:33
546  03/07/2007 00:45:53
547  27/04/2007 05:25:52

[548 rows x 1 columns]
```

```
</> Python
query = "SELECT created_at FROM Users Order By created_at ASC"
result = duckdb.sql(query).df()

print(result)

```

```
</> Python

Output:

       created_at
0      1177689952
1      1183464353
2      1184249013
3      1196090222
4      1198993923
..            ...
543    1486073840
544    1492614927
545 Sun Oct 23 18:23:37 +0000 2016
546 Tue Apr 07 19:36:11 +0000 2015
547 Tue May 25 16:02:26 +0000 2010

[548 rows x 1 columns]
27/04/2007 13:05:52
```

### Query B

Users with the highest number of followers were identified, indicating greater reach and influence.

```
</> Python

query = "SELECT followers_count FROM Users"
result = duckdb.sql(query).df()

print(result.describe())

result.describe()

```
```
</> Python

Output: 

| **Statistic**                | **Followers Count (Converted Value)** |
| ---------------------------- | ------------------------------------: |
| **Count**                    |                                   548 |
| **Mean**                     |                            163,433.90 |
| **Standard Deviation (Std)** |                          1,597,357.00 |
| **Minimum (Min)**            |                                     4 |
| **25% (1st Quartile)**       |                              8,960.25 |
| **50% (Median)**             |                             16,732.00 |
| **75% (3rd Quartile)**       |                             33,081.00 |
| **Maximum (Max)**            |                            31,712,580 |

```

### Query C

It was found that only 10 of the 540 users did not have a verified account; this points, to some extent, to the nature of state-run online channels—which are more easily and logically verified, given their active role within the U.S. legal framework—and confirms their function in establishing individual identity.

```
</> Python

query = "SELECT verified, Count(*) as how_many_users_are_verified From Users Group By verified"
result = duckdb.sql(query).df()

print(result)

```

```

</> Python

| **Verified** | **Number of Users** |
| :----------: | ------------------: |
|     True     |                 530 |
|     False    |                  18 |

```

### Query D

The distribution of users by location in the United States was determined (1 per location), revealing a geographically distributed representation.

```
</> Python

query = "SELECT location, Count(*) as location_count From Users Group By location"
result = duckdb.sql(query).df()

print(result)

```
```
</> Python

0         Boise, Idaho, USA              1
1       Howard, Pennsylvania             1
2          Schaumburg, IL                1
3          Sacramento, CA                1
4                     #NJ                1
..                    ...              ...
352          Oshkosh, WI                 1
353           Vermont/DC                 1
354              Indiana                 1
355      Friendswood, TX                 1
356       Banner Elk, NC                 1

```

### Additional Descriptive Statistics

Code to extract the mean, mode, population/sample standard deviation, range, and variance (for the columns where the necessary calculations could be performed).

 ### Conclusion about Descriptive Statics


```
</> Python

Users = Users.apply(pd.to_numeric, errors="coerce")

average = Users.mean(numeric_only=True)
print(avarege)

```

```
</> Python
| **Column Name**                    | **Mean (Converted Value)** |
| ---------------------------------- | -------------------------: |
| contributors_enabled               |                   0.000000 |
| created_at                         |              1,320,118,000 |
| default_profile                    |                   0.257299 |
| default_profile_image              |                   0.003650 |
| description                        |                        NaN |
| entities                           |                        NaN |
| favourites_count                   |                 413.192400 |
| follow_request_sent                |                   0.000000 |
| followers_count                    |             163,433.900000 |
| following                          |                   0.012774 |
| friends_count                      |               2,033.732000 |
| geo_enabled                        |                   0.516423 |
| has_extended_profile               |                   0.098540 |
| id                                 |     72,363,030,000,000,000 |
| id_str                             |     72,363,030,000,000,000 |
| is_translation_enabled             |                   0.027372 |
| is_translator                      |                   0.000000 |
| lang                               |                        NaN |
| listed_count                       |               1,340.648000 |
| location                           |                        NaN |
| name                               |                        NaN |
| notifications                      |                   0.000000 |
| profile_background_color           |               ∞ (Infinity) |
| profile_background_image_url       |                        NaN |
| profile_background_image_url_https |                        NaN |
| profile_background_tile            |                   0.136861 |
| profile_banner_url                 |                        NaN |
| profile_image_url                  |                        NaN |
| profile_image_url_https            |                        NaN |
| profile_link_color                 |               ∞ (Infinity) |
| profile_sidebar_border_color       |              33,680.180000 |
| profile_sidebar_fill_color         |              60,657.600000 |
| profile_text_color                 |               ∞ (Infinity) |
| profile_use_background_image       |                   0.753650 |
| protected                          |                   0.000000 |
| screen_name                        |                        NaN |
| statuses_count                     |               3,658.960000 |
| time_zone                          |                        NaN |
| translator_type                    |                        NaN |
| url                                |                        NaN |
| utc_offset                         |             -16,819.090000 |
| verified                           |                   0.967153 |
```
```

</> Python

mode = Users.mode().T

```

```

</> Python

Output(Explained):

| **Column Name**                    | **Mode Value** | **Practical Meaning**                                            |
| ---------------------------------- | -------------- | ---------------------------------------------------------------- |
| is_translation_enabled             | False          | Translation is disabled on most accounts.                        |
| is_translator                      | False          | Regular users (not official platform translators).               |
| lang                               | Non-numeric    | Text column (coerced to `NaN` by `pd.to_numeric`).               |
| listed_count                       | 307.0 / 351.0  | Tie (multimodal values).                                         |
| location                           | Non-numeric    | Text column (coerced to `NaN` by `pd.to_numeric`).               |
| name                               | Non-numeric    | Text column (coerced to `NaN` by `pd.to_numeric`).               |
| notifications                      | False          | Push notifications are disabled by default.                      |
| profile_background_color           | 0.0            | Hexadecimal color code incorrectly converted to a numeric value. |
| profile_background_image_url       | Non-numeric    | Image URLs (coerced to `NaN` by `pd.to_numeric`).                |
| profile_background_image_url_https | Non-numeric    | Image URLs (coerced to `NaN` by `pd.to_numeric`).                |
| profile_background_tile            | False          | Background image is not tiled.                                   |
| profile_banner_url                 | Non-numeric    | Image URLs (coerced to `NaN` by `pd.to_numeric`).                |
| profile_image_url                  | Non-numeric    | Image URLs (coerced to `NaN` by `pd.to_numeric`).                |
| profile_image_url_https            | Non-numeric    | Image URLs (coerced to `NaN` by `pd.to_numeric`).                |
| profile_link_color                 | 9999.0         | Hexadecimal color code incorrectly converted to a numeric value. |
| profile_sidebar_border_color       | 0.0            | Hexadecimal color code incorrectly converted to a numeric value. |
| profile_sidebar_fill_color         | 0.0            | Hexadecimal color code incorrectly converted to a numeric value. |
| profile_text_color                 | 333333.0       | Default hexadecimal text color converted to a numeric value.     |
| profile_use_background_image       | True           | Most profiles use a background image.                            |
| protected                          | False          | Most accounts are public (not protected).                        |
| screen_name                        | Non-numeric    | Usernames (coerced to `NaN` by `pd.to_numeric`).                 |
| statuses_count                     | 0.0 / 3583.0   | Tie (multimodal values).                                         |
| time_zone                          | Non-numeric    | Text column (coerced to `NaN` by `pd.to_numeric`).               |
| translator_type                    | Non-numeric    | Text column (coerced to `NaN` by `pd.to_numeric`).               |
| url                                | Non-numeric    | Website URLs (coerced to `NaN` by `pd.to_numeric`).              |
| utc_offset                         | -14400.0       | Most common UTC offset (UTC−4).                                  |
| verified                           | True           | The majority of accounts are verified.                           |


```

```

</> Python

amplitude = Users.var(numeric_only=True)

print(amplitude)

```

```
</> Python

Output(Explained):


| **Column Name**              |                 **Variance (Converted Value)** | **Practical Meaning**                                          |
| ---------------------------- | ---------------------------------------------: | -------------------------------------------------------------- |
| contributors_enabled         |                                       0.000000 | No variance (all values are identical).                        |
| default_profile              |                                       0.191446 | Variance of the default profile flag.                          |
| default_profile_image        |                                       0.003643 | Very low variance in the default profile image flag.           |
| favourites_count             |                                     931,517.30 | Variance of the number of favorites.                           |
| follow_request_sent          |                                       0.000000 | No variance (all values are identical).                        |
| followers_count              |                              2,555,155,000,000 | Variance of the number of followers.                           |
| following                    |                                       0.012634 | Variance of the following flag.                                |
| friends_count                |                                     39,418,760 | Variance of the number of friends.                             |
| geo_enabled                  |                                       0.250187 | Variance of the geolocation enabled flag.                      |
| has_extended_profile         |                                       0.088992 | Variance of the extended profile flag.                         |
| id                           | 53,463,280,000,000,000,000,000,000,000,000,000 | Variance of user IDs.                                          |
| id_str                       | 53,463,280,000,000,000,000,000,000,000,000,000 | Variance of user ID strings converted to numeric values.       |
| is_translation_enabled       |                                       0.026672 | Variance of the translation enabled flag.                      |
| is_translator                |                                       0.000000 | No variance (all values are identical).                        |
| listed_count                 |                                     12,727,690 | Variance of the number of public lists containing the account. |
| notifications                |                                       0.000000 | No variance (all values are identical).                        |
| profile_background_tile      |                                       0.118346 | Variance of the background tile flag.                          |
| profile_use_background_image |                                       0.186001 | Variance of the background image usage flag.                   |
| protected                    |                                       0.000000 | No variance (all accounts share the same protection status).   |
| statuses_count               |                                     18,141,410 | Variance of the number of posted statuses (tweets).            |
| utc_offset                   |                                     20,366,170 | Variance of the UTC offset values.                             |
| verified                     |                                       0.031826 | Variance of the verified account flag.                         |


```

```

</> Python

numericas = Users.select_dtypes(include=[np.number])

desvio = numericas.std()

print(desvio)

```
```

</> Python
favourites_count       9.651514e+02
followers_count        1.597357e+06
friends_count          6.278436e+03
id                     2.312213e+17
id_str                 2.312213e+17
listed_count           3.567588e+03
statuses_count         4.259273e+03
utc_offset             4.512889e+03
dtype: float64
```

## Beyond descriptive statistics

### Loading descriptive statistical data:

<img width="1281" height="572" alt="image" src="https://github.com/user-attachments/assets/8103cd19-20a3-45f5-b3d4-ea8c67fe7ad5" />

<img width="1235" height="536" alt="image" src="https://github.com/user-attachments/assets/c83a4073-4939-4311-8d23-1214772280dc" />

<img width="1097" height="592" alt="image" src="https://github.com/user-attachments/assets/3ebcdb2a-73f7-4295-b04a-d74e0f43f5bb" />

<img width="1049" height="463" alt="image" src="https://github.com/user-attachments/assets/de5a4d1e-4099-42de-975b-91aaa0935402" />

<img width="55" height="458" alt="image" src="https://github.com/user-attachments/assets/c0784043-c088-402e-a344-530f89bd071f" />

## Looking deeper

### Most recurring topics

converter step created_in

```

Tweets["created_at"] = pd.to_datetime(
    Tweets["created_at"],
    unit="s",
    utc=True
)

```
```

query = "SELECT created_at FROM Tweets"
resultado = duckdb.sql(query).df()

print(resultado)

```
```
          created_at
0    2008-08-04 14:28:51-03:00
1    2008-08-06 16:04:45-03:00
2    2008-08-06 17:35:36-03:00
3    2008-08-07 10:52:52-03:00
4    2008-08-07 12:12:05-03:00
...                        ...
1243365 2017-06-06 14:15:01-03:00
1243366 2017-06-06 14:15:03-03:00
1243367 2017-06-06 14:15:17-03:00
1243368 2017-06-06 14:15:57-03:00
1243369 2017-06-06 14:16:00-03:00

[1243370 rows x 1 columns]
```

Importing libraries
```

Output:

from IPython.display import display
from sklearn import datasets
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import pandasql as ps
from pandasql import *
from sqlalchemy import text
from sqlalchemy import sql
from sqlalchemy import create_engine
from pandasql import sqldf as pysqldf
import pandasql

```
Checking the column we will use
```
Tweets_sql = Tweets[["text"]]

pysqldf("SELECT text FROM Tweets_sql")
```

Selecting it in lowercase letters

```
text182 = pysqldf("SELECT (LOWER(text)) AS text FROM Tweets_sql")
```

Downloading stopwords

```
import nltk

nltk.download('stopwords')
```

Downloading the necessary punkt function

```
import nltk

nltk.download('punkt_tab')

```

Counting the most frequently used words and excluding invalid results

```
from nltk.tokenize import word_tokenize
import re

texto = " ".join(text182["text"].astype(str))

# Remove URLs
texto = re.sub(r"http\S+", "", texto)

# Remove mentions (@user)
texto = re.sub(r"@\w+", "", texto)

# Remove just the simbol #
texto = texto.replace("#", "")

# Remove just the simbol !
texto = texto.replace("!", "")

# Remove just the simbol -
texto = texto.replace("-", "")

# Remove just the simbol :
texto = texto.replace(":", "")

# Remove just the simbol ,
texto = texto.replace(",", "")

# Remove just the simbol ...
texto = texto.replace("...", "")

# Remove just the simbol &
texto = texto.replace("&", "")

# Remove just the simbol ?
texto = texto.replace("?", "")

# Remove just the simbol .
texto = texto.replace(".", "")

# Remove just the simbol (
texto = texto.replace("(", "")

# Remove just the simbol $
texto = texto.replace("$", "")

ruidos = {"rt", "s", "n't", "'", '"', "”", ")"}

words = word_tokenize(texto.lower())

# Filtrar as palavras
palavras_filtradas = [p for p in words if p not in stop_words and p not in ruidos]

#print(palavras_filtradas)

````

Leaving only the top 30

```
from collections import Counter

wordDict = Counter(palavras_filtradas)

top30 = wordDict.most_common(30)

print(top30)
```

Creating a chart

```
from wordcloud import WordCloud
import matplotlib.pyplot as plt

# Junta todas as palavras em um único texto
texto = " ".join(palavras_filtradas)

# Creat a word cloud
wordcloud = WordCloud(
    width=1000,
    height=600,
    background_color="white",
    max_words=100
).generate(texto)

# Exibition
plt.figure(figsize=(14, 8))
plt.imshow(wordcloud, interpolation="bilinear")
plt.axis("off")
plt.title("Word cloud from Tweets", fontsize=18)
plt.show()
```

New metric - Word Chart

Counting words year by year and removing useless ones (numerals/acronyms)

```
import re
from collections import Counter
from nltk.tokenize import word_tokenize
from nltk.corpus import stopwords
from wordcloud import WordCloud
import matplotlib.pyplot as plt

stop_words = set(stopwords.words("english"))
ruidos = {"rt", "s", "n't", "'", '"', ")", ";", "amp", "w/", "“", "”"}

def gerar_wordcloud(df, ano):
    
    texto = " ".join(df["text"].astype(str)).lower()
    
    # Remove URLs
    texto = re.sub(r"http\S+", "", texto)
    
    # Remove menções
    texto = re.sub(r"@\w+", "", texto)
    
    # Remove apenas o símbolo # (mantém o texto da hashtag)
    texto = texto.replace("#", "")
    
    # Tokenização
    palavras = word_tokenize(texto)
    
    # Filtragem
    palavras_filtradas = [
        p for p in palavras
        if p.isalpha()
        and p not in stop_words
        and p not in ruidos
    ]

    contador = Counter(palavras_filtradas)

    print(f"\n====== {ano} ======")
    print(contador.most_common(30))

    if len(contador) == 0:
        print("Nenhuma palavra encontrada.")
        return

    wc = WordCloud(
        width=1200,
        height=700,
        background_color="white",
        max_words=100
    ).generate_from_frequencies(contador)

    plt.figure(figsize=(15,8))
    plt.imshow(wc, interpolation="bilinear")
    plt.axis("off")
    plt.title(f"Word Cloud - {ano}", fontsize=18)
    plt.show()

```
Tweets["created_at"] = pd.to_datetime(Tweets["created_at"])

for ano in range(2008, 2018):
    
    df_ano = Tweets[Tweets["created_at"].dt.year == ano][["text"]]
    
    print(f"Tweets in {ano}: {len(df_ano)}")
    
    gerar_wordcloud(df_ano, ano)
```

    New metric – creating year-by-year word charts

### Sentiment Analysis

Creating a column that expresses the sentiment conveyed by the words, ranging from -1 to 1, and rounding the results to remove scientific notation

```
from textblob import TextBlob

Tweets["sentiment"] = Tweets["text"].apply(
    lambda text: TextBlob(str(text)).sentiment.polarity
)
```

```
Tweets["sentiment"] = Tweets["sentiment"].round(4)

```

```
Tweets_sql124 = Tweets[["text", "sentiment", "created_at"]].copy()

```

```
pysqldf ( "SELECT text, sentiment from Tweets_sql124 where sentiment >= 0.0 order by sentiment Desc " )

```

```

                                                     text  sentiment
0        The best example of 'circular fundraising' I'v...        1.0
1              just signed up and happy to be on board!        1.0
2                    @AlanBarber great to hear from you!        1.0
3        #tcot @kenblackwell - happy to be on board wit...        1.0
4                 @michelebachmann - welcome to twitter!        1.0
...                                                   ...        ...
1103547               👇🏽 Let's do it. https://t.co        0.0
1103548  @BetsyDeVosED How can you protect American stu...        0.0
1103549  @urbaninstitute @BrookingsInst @TaxPolicyCente...        0.0
1103550    Dismantling #DoddFrank returns us to the days ...        0.0
1103551  In the shadows of the #ComeyHearing, @HouseGOP...        0.0

[1103552 rows x 2 columns]

```

```
pysqldf ( "SELECT text, sentiment from Tweets_sql124 where sentiment < 0 order by sentiment Desc " )
```

```
Output:

                                                     text  sentiment
0        RT @TeamJohnKasich: VERY easy to imagine after...    -0.0001
1            Discussing major reforms to the #Pentagon in #...    -0.0004
2         Surprise, surprise. New @UCSUSA report shows 8...    -0.0004
3         RT @NormEisen: major point folks r missing is ...    -0.0004
4        These major investments don't happen overnight...    -0.0006
...                                                   ...        ...
139813   .@POTUS has announced plans to privatize US Ai...    -1.0000
139814        National concealed-carry reciprocity is a terr...    -1.0000
139815       The Republican health care bill guts the Medic...    -1.0000
139816     The Trump Admin's plans to gut Medicaid would ...    -1.0000
139817       GOP health plan cuts Medicaid. Devastating for...    -1.0000

[139818 rows x 2 columns]

```

New metric - Histogram of the 'sentiments' column

New metric—creating histograms that express the sentiment conveyed by the words year by year, ranging from -1 to 1. And rounding the results to remove scientific notation
conferir

```
Tweets_sql125 = Tweets[["text", "sentiment", "created_at"]].copy()

import pandas as pd
from textblob import TextBlob
import matplotlib.pyplot as plt

# crieted the column year
Tweets_sql125["year"] = Tweets_sql125["created_at"].dt.year

# calculeted the sentiment
Tweets_sql125["sentiment"] = Tweets_sql125["text"].apply(
    lambda x: TextBlob(str(x)).sentiment.polarity
)

#one histogram by year
for year, temp_df in Tweets_sql125.groupby("year"):
    
    print(f"Tweets from {year}")
    
    temp_df["sentiment"].hist(bins=30)
    plt.title(f"Sentiment - {year}")
    plt.xlabel("Polarity")
    plt.ylabel("Frequency")
    plt.show()

```

### Predictions about the tweets number tweeted

Counting the total number of tweets
```
pysqldf ( "SELECT Count(text) as total_tweets from Tweets_sql125" )
```

```
Output:

   total_tweets
0       1243370

```

Counting the tweets year by year

```

pysqldf("""
SELECT year, COUNT(text) AS total_tweets_by_year
FROM Tweets_sql125
GROUP BY year
ORDER BY year
""")

```

```

Output:

   year  total_tweets_by_year
0  2008                   112
1  2009                  8234
2  2010                 13763
3  2011                 35163
4  2012                 50791
5  2013                124439
6  2014                168308
7  2015                258256
8  2016                354942
9  2017                229362

```

```
dados = (
    Tweets_sql125.groupby("year")
    .size()
    .reset_index(name="total_tweets")
)

print(dados)
```
```
   year  total_tweets
0  2008           112
1  2009          8234
2  2010         13763
3  2011         35163
4  2012         50791
5  2013        124439
6  2014        168308
7  2015        258256
8  2016        354942
9  2017        229362
```

By performing a linear regression on the number of tweets per year—that is, plotting a red linear trend line based on the results to project future outcomes—we can observe the trend. Note that linear regression is a machine learning process.

```
from sklearn.linear_model import LinearRegression
import matplotlib.pyplot as plt
import numpy as np

# X = anos
X = dados[["year"]]

# y = quantidade de tweets
y = dados["total_tweets"]

# Ajuste do modelo
modelo = LinearRegression()
modelo.fit(X, y)

# Valores previstos para os anos existentes
dados["previsto"] = modelo.predict(X)

# Anos futuros
anos_futuros = np.array([[2018], [2019], [2020]])
previsao = modelo.predict(anos_futuros)

# Junta anos históricos + futuros para desenhar uma única reta
anos_todos = np.vstack([X.values, anos_futuros])
reta_toda = modelo.predict(anos_todos)

# Gráfico
plt.figure(figsize=(10,6))

# Dados reais
plt.scatter(X, y, color="blue", s=60, label="Dados reais")

# Reta de regressão (histórico + futuro)
plt.plot(anos_todos, reta_toda, color="red", linewidth=2, label="Regressão Linear")

# Pontos previstos
plt.scatter(
    anos_futuros,
    previsao,
    color="green",
    marker="X",
    s=120,
    label="Previsão"
)

# Escreve o valor previsto ao lado dos pontos
for ano, valor in zip(anos_futuros.flatten(), previsao):
    plt.text(ano, valor, f"{valor:.0f}", fontsize=10,
             ha="left", va="bottom")

plt.xlabel("Ano")
plt.ylabel("Quantidade de Tweets")
plt.title("Regressão Linear e Previsão de Tweets")
plt.grid(True)
plt.legend()

plt.show()

print("Previsões:")
for ano, valor in zip(anos_futuros.flatten(), previsao):
    print(f"{ano}: {valor:.0f} tweets")


New metric: Linear regression and Tweet forecasting

Repetir...

R² of the linear regression above

```
r2 = modelo.score(X, y)
print("R² =", r2)

#R² close to 1 (0.9 or higher): the relationship is strongly linear.
#R² between 0.7 and 0.9: linear regression may be acceptable.
#R² below 0.5: the relationship is likely not well explained by a straight line, and another model (polynomial, exponential, time series, etc.) might be more suitable.

#In addition to R², it is important to visualize the data. If the points form an approximately straight line-either rising or falling-linear regression makes sense. If there are curves, cycles, or abrupt changes, the relationship is likely not linear.

```

```
R² = 0.5852164739733133
```

Performing a Pearson analysis of the number of tweets per year (plotting a linear regression line with a slope based on the results)

```

dados1 = pysqldf("""
SELECT year, COUNT(*) AS total_tweets
FROM Tweets_sql125
GROUP BY year
ORDER BY year
""")

correlacao = dados1["year"].corr(dados1["total_tweets"])

print(f"Correlação de Pearson: {correlacao:.4f}")

```

New Metric - Pearson Correlation

<img width="761" height="470" alt="image" src="https://github.com/user-attachments/assets/00b9fef9-17d4-4a11-afd5-f20c1104b774" />

### Predictions about the most reccurent topics tweeted

Performing Pearson analysis and generating Pearson plots for some of the most frequently counted words

1 - Student

```
print(f"r = {r:.3f}")
print(f"p = {p:.3f}")

# Gráfico
plt.scatter(dados["year"], dados["freq_student"])
plt.plot(
    dados["year"],
    np.poly1d(np.polyfit(dados["year"], dados["freq_student"], 1))(dados["year"]),
    color="red"
)

plt.xlabel("Ano")
plt.ylabel("Frequência da palavra 'student'")
plt.title(f"Correlação de Pearson (r = {r:.3f})")
plt.grid(True)
plt.show()
```

```

Output:

r = 0.886
```


New metric – Student (Pearson correlation)

<img width="713" height="472" alt="image" src="https://github.com/user-attachments/assets/c746955a-c0b0-4f80-a581-8bec0359cf08" />

2 - Small Businesses (Pearson Correlation) / New Metric

```
import pandas as pd
from scipy.stats import pearsonr
import matplotlib.pyplot as plt

# Conta quantas vezes "small business" aparece por ano
dados = []

for ano in range(2008, 2018):
    df_ano = Tweets[Tweets["created_at"].dt.year == ano]
    
    texto = " ".join(df_ano["text"].astype(str)).lower()
    
    freq = texto.count("small business")
    
    dados.append([ano, freq])

dados = pd.DataFrame(dados, columns=["year", "freq_small business"])

# Correlação de Pearson
r, p = pearsonr(dados["year"], dados["freq_small business"])

print(f"r = {r:.3f}")
print(f"p = {p:.3f}")

# Gráfico
plt.scatter(dados["year"], dados["freq_small business"])
plt.plot(
    dados["year"],
    np.poly1d(np.polyfit(dados["year"], dados["freq_small business"], 1))(dados["year"]),
    color="red"
)

plt.xlabel("Ano")
plt.ylabel("Frequência da palavra 'small business'")
plt.title(f"Correlação de Pearson (r = {r:.3f})")
plt.grid(True)
plt.show()

```

```
Output:
```

<img width="662" height="485" alt="image" src="https://github.com/user-attachments/assets/0e72bc3c-2776-4374-9718-1ea9088d1f9b" />

3 - Congress (Pearson Correlation) / New Metric
```
import pandas as pd
from scipy.stats import pearsonr
import matplotlib.pyplot as plt

# Conta quantas vezes "congress" aparece por ano
dados = []

for ano in range(2008, 2018):
    df_ano = Tweets[Tweets["created_at"].dt.year == ano]
    
    texto = " ".join(df_ano["text"].astype(str)).lower()
    
    freq = texto.count("congress")
    
    dados.append([ano, freq])

dados = pd.DataFrame(dados, columns=["year", "freq_congress"])

# Correlação de Pearson
r, p = pearsonr(dados["year"], dados["freq_congress"])

print(f"r = {r:.3f}")
print(f"p = {p:.3f}")

# Gráfico
plt.scatter(dados["year"], dados["freq_congress"])
plt.plot(
    dados["year"],
    np.poly1d(np.polyfit(dados["year"], dados["freq_congress"], 1))(dados["year"]),
    color="red"
)

plt.xlabel("Ano")
plt.ylabel("Frequência da palavra 'congress'")

    dados.append([ano, frequencia])

dados = pd.DataFrame(dados, columns=["Ano", "Frequencia"])

# Regressão Linear
X = dados[["Ano"]]
y = dados["Frequencia"]

modelo = LinearRegression()
modelo.fit(X, y)

dados["Previsto"] = modelo.predict(X)

r2 = r2_score(y, dados["Previsto"])

# Previsão para os próximos 3 anos
anos_futuros = pd.DataFrame({"Ano": [2018, 2019, 2020]})
previsao = modelo.predict(anos_futuros)

# Resultados
print("=" * 60)
print(f"Palavra: {palavra.upper()}")
print(f"Coeficiente angular: {modelo.coef_[0]:.4f}")
print(f"Intercepto: {modelo.intercept_:.2f}")
print(f"R²: {r2:.4f}")

print("\nPrevisões:")
for ano, valor in zip(anos_futuros["Ano"], previsao):
    print(f"{ano}: {valor:.2f}")

# Gráfico
plt.figure(figsize=(10,6))

plt.scatter(
    dados["Ano"],
    dados["Frequencia"],
    s=70,
    label="Dados observados"
)

plt.plot(
    dados["Ano"],
    dados["Previsto"],
    color="red",
    linewidth=2,
    label="Regressão Linear"
)

plt.scatter(
    anos_futuros["Ano"],
    previsao,
    marker="x",
    s=90,
    label="Previsão"
)

plt.title(f"Regressão Linear - Palavra '{palavra}'")
plt.xlabel("Ano")
plt.ylabel("Frequência")
plt.grid(True)
plt.legend()

plt.show()


```
3 - Creating linear regression and linear regression plots for the words (congress, violence, health, student, and house)

New Metric - Linear regression for the words (congress, violence, health, student, and house)

```
Output:
```

```
Palavra: CONGRESS
Coeficiente angular: 752.9697
Intercepto: -1512869.12
R²: 0.7825

Previsões:
2018: 6623.73
2019: 7376.70
2020: 8129.67
```
```
Palavra: VIOLENCE
Coeficiente angular: 170.4606
Intercepto: -342547.97
R²: 0.4738

Previsões:
2018: 1441.53
2019: 1611.99
2020: 1782.45
```

<img width="651" height="412" alt="image" src="https://github.com/user-attachments/assets/80e21852-db4b-4ccc-a7e9-b4a72c89bcb1" />

```
============================================================
Palavra: HEALTH
Coeficiente angular: 937.2909
Intercepto: -1883333.05
R²: 0.8390

Previsões:
2018: 8120.00
2019: 9057.29
2020: 9994.58
```

<img width="693" height="437" alt="image" src="https://github.com/user-attachments/assets/cb6fade1-ed73-49ef-a666-64ed9502b456" />

```
============================================================
Palavra: STUDENT
Coeficiente angular: 170.6909
Intercepto: -342876.25
R²: 0.6927

Previsões:
2018: 1578.00
2019: 1748.69
2020: 1919.38
```

<img width="716" height="450" alt="image" src="https://github.com/user-attachments/assets/543ab284-d483-45fa-9a25-36e8d006796e" />
 
```
============================================================
Palavra: HOUSE
Coeficiente angular: 1091.8061
Intercepto: -2192924.70
R²: 0.8195

Previsões:
2018: 10339.93
2019: 11431.74
2020: 12523.55
```

<img width="850" height="501" alt="image" src="https://github.com/user-attachments/assets/8df0060c-15fe-42a7-97c6-0be7c8e8c7e4" />

## Conclusion

In a way, what surprises me most is that all our hypotheses proved to be valid.

[ Hypotheses ]

1- As shown by the descriptive statistics, the users who commented the most indeed held more significant weight/influence. By going beyond basic descriptive statistics, we were able to establish a visible metric for this—a word cloud representing the most frequently repeated words in their descriptions.

2- As observed in the descriptive statistics, the primary connections involved members of Congress, with the strength of the relationship correlating to their level of importance. By going beyond descriptive statistics, we created a word cloud displaying the most frequently repeated words in the profiles of the most retweeted members.

3- We found that the topics most frequently tweeted about by Congress members were indeed controversial and engaging subjects, such as recent events (current news) and gun violence; legislative matters affecting specific groups (families, households, students, small businesses); and topics directly related to Congress itself.


In progresss...
