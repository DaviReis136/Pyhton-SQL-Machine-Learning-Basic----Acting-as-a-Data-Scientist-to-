# Resum 

My project centers on `Lobbyists4America`, <ins>a firm operating in the legislative sector that aims to provide valuable information to its clients—the company's target audience</ins>. These clients, in turn, seek to influence legislation in the United States. Achieving this requires a strategic approach to gathering relevant information for them. To implement this strategy, they have hired me <ins>as a data scientist to analyze Congressional tweets from 2008 to 2017</ins>, shedding light on key issues, relationships, and members of Congress. Consequently, the target audience for the proposed solution is the same group that requested it: Lobbyists4America’s own clients. However, there is another group that—while not the primary focus of my project—might also be interested in the solution addressing the issues raised by Lobbyists4America. This group includes media outlets attracted to major topics discussed by Congress (especially attention-grabbing subjects like the implementation of specific laws) and the professionals within that sphere—journalists, reporters, and the outlets' respective audiences—as well as the parties upon whom Lobbyists4America’s clients sought to exert influence based on the issues highlighted by the Congressional data.


# Contents​

> Review of Questions to Answer / Hypothesis / Approach
  + Review of Questions to Answer
  + Review of Questions to Hypothesis
  + Review of Questions to Hypothesis
> Exploratory analysis

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
import pandas as pd 
with open('tweets.json', encoding='utf-8') as inputfile: 
df = pd.read_json(inputfile, lines=True) 
df.to_csv('tweets.csv', encoding='utf-8', index=False) 
And for the other dataset: 
import pandas as pd 
with open('user.json', encoding='utf-8') as inputfile: 
df = pd.read_json(inputfile, lines=True) 
df.to_csv('user.csv', encoding='utf-8', index=False)



 <img width="525" height="176" alt="Captura de tela 2026-07-01 155740" src="https://github.com/user-attachments/assets/334290e8-561e-4bca-be5e-dc67c06915fe" />

<img width="511" height="201" alt="Captura de tela 2026-07-01 160230" src="https://github.com/user-attachments/assets/3d1506dc-d45d-4909-9260-439f2ee5def8" />


After this process, it is time to import the data into the software. In SQLiteStudio, once I have checked the columns in the CSV file, I will populate the columns in SQLiteStudio using the same names and quantity; then, I will import the data into the table created in SQLiteStudio:

<img width="1347" height="767" alt="Captura de tela 2026-07-01 175729" src="https://github.com/user-attachments/assets/d9603504-409b-49a7-86b8-6cfec89b0e1a" />

<img width="1355" height="756" alt="Captura de tela 2026-07-01 193439" src="https://github.com/user-attachments/assets/dbbc8d9c-58ac-4c77-ab03-3ab567387984" />

To begin my exploratory analysis, I decided to find a column that established a link between the tweets table and the users table. After examining the columns, I noticed that the tweets table had a column named `user_id`, which seemed like an obvious connection point between the two; in the users table, I found the `id` column, which appeared to be the one referenced by the tweets table, so I ran a query to confirm.


<img width="560" height="293" alt="Captura de tela 2026-07-01 200806" src="https://github.com/user-attachments/assets/99f0411d-253a-4b7b-8ced-d8cd26b645de" />

<img width="609" height="475" alt="Captura de tela 2026-07-01 200917" src="https://github.com/user-attachments/assets/daa8e235-7861-42a3-8518-c985e1ca0531" />

<img width="705" height="178" alt="Captura de tela 2026-07-01 202225" src="https://github.com/user-attachments/assets/5ff17b3a-fe2e-4a26-bbcb-8cb8d58dd3a8" />

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


in progress...
