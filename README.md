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

## 

<img width="846" height="447" alt="image" src="https://github.com/user-attachments/assets/e9836a02-d500-486f-971b-e87805d2367c" />

<img width="829" height="429" alt="image" src="https://github.com/user-attachments/assets/c4b5c463-8d9f-49a6-8a89-a811c13bc527" />

##

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

| Estatística       | created_at          | favorite_count |                      id |   in_reply_to_status_id | in_reply_to_user_id | retweet_count |            user_id |        quoted_status_id |
| :---------------- | :------------------ | -------------: | ----------------------: | ----------------------: | ------------------: | ------------: | -----------------: | ----------------------: |
| **Count**         | 1,243,370           |      1,243,370 |               1,243,370 |                  54,146 |              65,411 |     1,243,370 |          1,243,370 |                  56,418 |
| **Mean**          | 2015-06-11 21:10:00 |         200.85 | 609,659,600,000,000,000 | 662,478,400,000,000,000 |  30,553,190,000,000 |        190.06 | 13,974,050,000,000 | 775,152,900,000,000,000 |
| **Std**           | 52,263,240          |       3,545.41 | 214,092,500,000,000,000 | 210,676,800,000,000,000 | 152,843,500,000,000 |      9,944.39 | 10,533,830,000,000 |   7,897,866,000,000,000 |
| **Min**           | 2008-08-04 17:28:51 |           0.00 |              87,741,860 |           1,059,864,000 |                  20 |          0.00 |          5,558,312 |              90,957,760 |
| **25%**           | 2014-06-14 02:40:00 |           0.00 | 476,795,200,000,000,000 | 552,497,700,000,000,000 |          30,380,230 |          1.00 |         33,750,800 | 725,045,100,000,000,000 |
| **50% (Mediana)** | 2015-11-04 16:50:00 |           2.00 | 662,381,800,000,000,000 | 739,907,400,000,000,000 |         204,905,600 |          4.00 |        234,022,300 | 794,551,000,000,000,000 |
| **75%**           | 2016-10-02 02:30:00 |           8.00 | 781,241,600,000,000,000 | 831,621,200,000,000,000 |         622,731,300 |         10.00 |         99,315,300 | 839,266,200,000,000,000 |
| **Max**           | 2017-06-06 17:16:00 |     984,832.00 | 872,140,000,000,000,000 | 872,139,400,000,000,000 | 866,745,300,000,000 |  3,637,896.00 | 85,471,510,000,000 | 872,138,300,000,000,000 |

```

<img width="886" height="280" alt="image" src="https://github.com/user-attachments/assets/f0e368dd-0dec-41b5-96ae-e3fab5ca4cac" />



-desativar notação cíentifica
pd.set_option('display.float_format', lambda x: '%.2f' % x)

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
resultado = duckdb.sql(query).df()
print(resultado)
```

```
</> Python
|        Nº | Data Formatada      |
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
query = "SELECT make_timestamp(CAST(created_at AS BIGINT) * 1000000) AS data_formatada FROM Tweets ORDER BY created_at ASC" resultado = duckdb.sql(query).df() print(resultado) então me mostra como sairia o resultado então
```

```
</> Python
|        Nº | Data Formatada      |
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
resultado = duckdb.sql(query).df()

print(resultado)

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
resultado = duckdb.sql(query).df()

print(resultado)
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

Output:

+-----------------------------+------------------------------------+

| Column Name                 | Converted Value / Meaning          |
+-----------------------------+------------------------------------+

| contributors                | NaN (Null / Empty)                 |
| coordinates                 | NaN (Null / Empty)                 |
| created_at                  | 2015-06-11 21:10:00 (Average Date) |
| display_text_range          | NaN (Null / Empty)                 |
| entities                    | NaN (Null / Empty)                 |
| favorite_count              | 200.85 (Average likes per tweet)   |
| favorited                   | 0.00 (Percentage close to 0)       |
| geo                         | NaN (Null / Empty)                 |
| id                          | 609659600000000000                 |
| id_str                      | 609659600000000000                 |
| in_reply_to_screen_name     | 14369.80                           |
| in_reply_to_status_id       | 662478400000000000                 |
| in_reply_to_status_id_str   | 662478400000000000                 |
| in_reply_to_user_id         | 30553190000000                     |
| in_reply_to_user_id_str     | 30553190000000                     |
| is_quote_status             | 0.05 (4.61% of all tweets)         |
| lang                        | NaN (Null / Empty)                 |
| place                       | NaN (Null / Empty)                 |
| retweet_count               | 190.06 (Average retweets per tweet)|
| retweeted                   | 0.00 (Exactly 0%)                  |
| screen_name                 | NaN (Null / Empty)                 |
| source                      | NaN (Null / Empty)                 |
| text                        | 45.38 (Average text length)        |
| truncated                   | 0.00 (Exactly 0%)                  |
+-----------------------------+------------------------------------+

```

in progress...


```
</> Python

```
