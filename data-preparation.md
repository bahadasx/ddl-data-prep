# Data Preparation for Entity Resolution

*by Sasan Bahadaran*

## Introduction

### Data Preparation and Data Science

Data preparation refers to the act of manipulating ones data to transform it into a suitable form for further analysis and processing.  This process involves many different tasks and requires understanding of what manipulations are necessary for a particular dataset, as well as what the goal is in terms of how you plan to process the data further.  Some of the tasks that can be part of a data preparation for a particular project include data cleaning, normalization, and imputation. 

Data preparation is a part of many data related projects, but specifically in terms of Data Science, it is said that data preparation accounts for about 60% of the time spent on a project.  Considering the percentage of time spent on this task, you can conclude that is the most crucial part of the Data Science process.  Without proper data preparation, you cannot expect the results of your analysis very accurate results.  The principal of "garbage in, garbage out" is very applicable in this context.  In this blog we will be discussing the basics of data preparation, some techniques that can be applied to a sample dataset, and a comparison of performing Entity Resolution with and without performing proper data preparation.

### Entity Resolution

For an explanation of Entity Resolution, please refer [here](https://github.com/krossetti/DDL-DC/blob/master/entity-resolution-basics.md#what-is-entity-resolution) 

In this blog, our focus will be specifically on record linkage and canonicalization of data.

### Dedupe

For an introduction to Dedupe, please refer [here](https://github.com/krossetti/DDL-DC/blob/master/entity-resolution-basics.md#about-dedupe)

## Methods of Data Preparation

### Data Normalization

According to [wikipedia](https://en.wikipedia.org/wiki/Canonical_form#Computing), the reduction of data to any kind of canonical form is commonly called data normalization.  In the context of Entity Resolution, this is the process of disambiguating or standardizing the data, which will help improve matching.  Below are some examples of normalization techniques:

* Convert to all lower case

It is helpful to have all text standardized to the same case prior to doing comparison.  Doing is helpful so all characters are standardized, but also can improve the performance of any matching processes.

* Remove whitespace

Some of the techniques mentioned above, such as standardizing abbreviations and removing spelling errors may seem like common sense when it comes to cleaning up your data in preparation for entity resolution, but how do some of the other aforementioned techniques such as removing whitespace help?  Some metrics that can be used for entity resolution are affected by whitespace, such as Levenshtein (or edit distance).  Levenshtein distance calculates the minimum number of edits necessary to change a word into another word.  Lets look at an example of whitespace can affect this score.

Using the levenshtein function that is part of the fuzzystrmatch module in Postgres we can run the following query comparing two words - "GUMBO" and "GAMBOL":
```
test=# select levenshtein ('GUMBO', 'GAMBOL'); 
-------------
           2
(1 row) 
```

The number of edits necessary to change "GUMBO" to "GAMBOL" is 2.  What happens if one of the words has a space in it in the database?

```
test=# select levenshtein ('GUMBO', ' GAMBOL ');
 levenshtein 
-------------
           4
(1 row)
```

In this example "GAMBOL" has been changed to " GAMBOL ".  The spaces on the left and right side mean that the edit score has increased.  Let's try another example:

```
test=# select levenshtein ('GUMBO', 'GA MBOL');
 levenshtein 
-------------
           3
(1 row)
```

* Standardize abbreviations

Sometimes simple issues with text can prevent finding good matches.  This includes standardization of abbreviations.  Some common examples of text that can be standardized include:
- Business entities (Inc, Ltd, Corp)
- Titles - (Dr, Prof, Capt, CTO)
- Address related fields (Dr, Ave)
- Dates - (MMDDYYYY)

The decision can be made to either expand or contract abbreviations.  Choosing one or the other will depend on your particular dataset, but usually, shortening abbreviations is a better choice since it creates less characters of text to process for comparison.

* remove spelling errors

Spelling errors can obviously lead to missed matches for data, so one thing you can do is to use a tool for spell checking prior to doing any Entity Resolution.  This can be done in many ways including using a program such as MS Word or Excel, using Google's "Did you Mean?" API, or writing a script in a language such as Python using a library like [NLTK](http://www.nltk.org).

* Remove punctuation




### Schema Normalization

Examples include:
* Match attribute names (title -> name)
* Split compound attributes (full name -> first, last)
* Segment records from raw text



## Tools for Data Preparation

As far as how to implement your strategy for data preparation there are many different methods.  Below are some various tools you can use:

* [OpenRefine](http://openrefine.org/) - This is a free, open source tool that used to be owned by Google with many methods for workkng with messy data.

* Regular Expressions - Regular Expressions, or regex, allow you to define patterns that can be used to flag instances of text that match that pattern.

(maybe this should be removed or added to line below)
* SQL scripts - Whether your data resides in a database or not, one way of preparing your data for entity resolution is to apply your prepping techniques using some SQL scripts.

* Scripts using various languages (e.g., Python, R) and libraries for cleaning data (e.g., Pandas) - Using langauges such as Python and R, you can create scripts that will apply your preparation techniques to the data.  There are many libraries available for these tasks for various languages such as [Pandas](http://pandas.pydata.org/) for Python.

* Excel - Being that many people use Excel, I thought it is worth mentioning that you can clean up your data using techniques in Excel as well.

## Our Sample Dataset

**Add a business case for deduping this data.  Mention the context of business vs establishment.

The sample dataset I will be using in this post comes from [OSHA](https://www.osha.gov) (Occupational Safety and Health Administration) and can be obtained from the Department of Labor [Data Enforcement website](http://enforcedata.dol.gov/views/data_summary.php).  It is a CSV file containing records of safety inspections performed by OSHA inspectors at various times.  When inspectors are conducting these safety inspections, they are manually entering all information, which leads to many errors and inconsistencies.  Some of the issues with this dataset include:
* no unique identifier for business
* non-standardized entry of business names
* Random punctuation
* Spelling errors
* Errors with addresses
* Unwanted characters?

**include table with a few examples of the issues with the data.

## Testing the Effect of Data Preparation on Canonicalization of Entities

In order to test our theory that good data preparation is helpful for performing effective entity resolution, we are going to perform a test using our sample dataset and the Dedupe library.  For our tests, we will be setting up a PostgreSQL database with our data. 

###OSX
PostgreSQL is installed by default on OSX 10.7 or later.  The easiest way to get setup with PostgreSQL on OSX is to use [Postgres.app](http://postgresapp.com).  This will install version 9.5.3 of PostgreSQL along with some other useful utilities.

(Include instructions on how to start DB for each one)

###Windows

###Linux(Ubuntu)

Now that we have installed we will need to load the sample dataset.  A script(sample_data.sql) is included.  (*include command for loading SQL script)

*include instructions on how to query database to check for data (select count?)

First, we will run our sample dataset through Dedupe without any data preparation.

**Do active learning

**Do clustering

**Show results

Next, we will run our sample dataset through Dedupe again.  This time though, we will perform some data preparation to clean up the data a bit.

Perform data prep on file

Do active learning

Do clustering

Show results

How can we visualize these results?

## Conclusion

## Additional Readings

