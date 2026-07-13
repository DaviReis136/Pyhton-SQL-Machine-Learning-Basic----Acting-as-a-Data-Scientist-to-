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

After this process, it is time to import the data into the software. In SQLiteStudio, once I have checked the columns in the CSV file, I will populate the columns in SQLiteStudio using the same names and quantity; then, I will import the data into the table created in SQLiteStudio:

<img width="1347" height="767" alt="Captura de tela 2026-07-01 175729" src="https://github.com/user-attachments/assets/d9603504-409b-49a7-86b8-6cfec89b0e1a" />

<img width="1355" height="756" alt="Captura de tela 2026-07-01 193439" src="https://github.com/user-attachments/assets/dbbc8d9c-58ac-4c77-ab03-3ab567387984" />

To begin my exploratory analysis, I decided to find a column that established a link between the tweets table and the users table. After examining the columns, I noticed that the tweets table had a column named `user_id`, which seemed like an obvious connection point between the two; in the users table, I found the `id` column, which appeared to be the one referenced by the tweets table, so I ran a query to confirm.

```
</> SQL

SELECT tweets.user_id,
       users.id,
       users.name
FROM tweets, users;

```

<img width="609" height="475" alt="Captura de tela 2026-07-01 200917" src="https://github.com/user-attachments/assets/daa8e235-7861-42a3-8518-c985e1ca0531" />

```
</> SQL

SELECT tweets.user_id AS user_id_from_tweet,
       users.id AS user_id_from_users,
       users.name
FROM tweets
LEFT JOIN users
ON tweets.user_id = users.id;

```

<img width="673" height="577" alt="Captura de tela 2026-07-01 202212" src="https://github.com/user-attachments/assets/61352fce-e628-492d-a9b3-6ba0ab291708" />

Now that I have confirmed the hypothesis, I am using them as foreign keys.
After that, I decided to verify a piece of information requested by the company—an analysis of tweets from 2008 to 2017—and realized that the data in the `tweets` table requires no trimming or additions, given that the creation date of the first tweet is in 2008 and the last is in 2017.

<img width="838" height="458" alt="Captura de tela 2026-07-01 202336" src="https://github.com/user-attachments/assets/3428cf71-0d65-41f3-9451-6fc34db248a0" />

<img width="895" height="479" alt="Captura de tela 2026-07-01 202428" src="https://github.com/user-attachments/assets/dedbde74-9e70-482c-a6e0-0dc157518896" />

After that, I simply needed to confirm the absence of duplicate records. To do this, I selected columns in both tables where duplicates should not exist—specifically the `id` column, as it serves to uniquely identify each record. Since there were no repetitions, no rows were duplicated. I then counted the records individually using the `GROUP BY` and `COUNT` functions and verified that everything was correct, as no record count exceeded one.

<img width="508" align = "left" height="229" alt="Captura de tela 2026-07-01 210354" src="https://github.com/user-attachments/assets/9caa184e-d92b-4305-9047-da7afa74096f" />

<img width="432" height="561" align = "right" alt="Captura de tela 2026-07-01 210305" src="https://github.com/user-attachments/assets/5687d98b-fe6b-4cd8-92a7-f9fcc41f9abe" />

##

<img width="568" height="321" alt="Captura de tela 2026-07-01 210405" src="https://github.com/user-attachments/assets/385c7bcf-0bfd-45da-bb29-d0db3a446e9c" />

<img width="447" height="311" alt="Captura de tela 2026-07-01 210417" src="https://github.com/user-attachments/assets/39821d05-d329-42ce-b9a1-99d04f5d7bc4" />

##

<img width="499" height="199" alt="Captura de tela 2026-07-01 210615" src="https://github.com/user-attachments/assets/4c7aee3d-ed02-441b-90a5-832c1cf14212" />

<img width="368" height="427" alt="Captura de tela 2026-07-01 210544" src="https://github.com/user-attachments/assets/b8b0f248-611f-4090-819d-00d89ef06df3" />

##

<img width="434" height="206" alt="Captura de tela 2026-07-01 210723" src="https://github.com/user-attachments/assets/2b62ea28-0ff8-4d43-8166-c52536c30b05" />

<img width="375" height="126" alt="Captura de tela 2026-07-01 210745" src="https://github.com/user-attachments/assets/c8afe07d-e582-4421-a81e-4c1f36a25968" />

Since I know which columns contain no duplicate records—and indeed should not have any—I will select them as primary keys. 
After that, I examine the records in each column to classify its data type. 
Example:

<img width="477" height="278" alt="image" src="https://github.com/user-attachments/assets/5b01ec48-5d9e-4195-969f-11f33d0387cf" />

<img width="197" height="25" alt="Captura de tela 2026-07-02 113602" src="https://github.com/user-attachments/assets/9f9dbc67-8f47-404d-9b39-0753bda3d88c" />

## Descriptive Stats


The primary libraries were imported to prepare the analysis environment.

in progress...

### Tweets Table

#### Import 

<img width="508" height="352" alt="image" src="https://github.com/user-attachments/assets/e8398a2a-7d0b-4de8-9642-89c9be40e027" />

### Data loading process

The data were loaded from a JSON file using the standard read function from the pandas library, excluding a row that proved to be corrupted.

<img width="683" height="443" alt="image" src="https://github.com/user-attachments/assets/01a91baa-85ae-44b4-adf5-a21c275a9c0b" />

### Import 

Next, secondary supporting libraries were imported.

<img width="240" height="204" alt="image" src="https://github.com/user-attachments/assets/0ef7ea49-92db-45f5-9d87-adfe7698b7bc" />

### Extracting information

A general inspection of the dataset was conducted, identifying columns, non-null values, and data types.

<img width="739" height="625" alt="image" src="https://github.com/user-attachments/assets/9b8d20bb-8fa5-4247-886f-608dc7780792" />
<img width="817" height="480" alt="image" src="https://github.com/user-attachments/assets/b1573227-a69c-4ba8-8f1c-5ea5f9406e3e" />

### Obtaining descriptive statistics

Descriptive statistics were obtained, including count, mean, minimum, maximum, and quartiles.

<img width="886" height="280" alt="image" src="https://github.com/user-attachments/assets/f0e368dd-0dec-41b5-96ae-e3fab5ca4cac" />


### Function to convert scientific notation

### Import DuckDB

The DuckDB library was imported to enable SQL queries.



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
