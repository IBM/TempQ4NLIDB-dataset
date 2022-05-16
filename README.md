# Dataset TempQ4NLIDB for the paper "Tackling Temporal Aspect in Natural Language Interface to Database"

This dataset TempQ4NLIDB consists of the following contents:

1) Two accompanied databases Warehouse (WH) and Human Resource (HR) compatible with SQLite.

2) Temporal questions and associated SQL queries for each database. There are 3 variants: Original temporal questions, Full-DA questions, and Partial-DA questions.

3) Non-temporal questions and associated SQL queries for each database.

## Data format:

Every data point consists of one natural language question and one associated SQL query regardless if it is a temporal or a non-temporal question.

### Temporal questions

There are three variants of temporal questions:

1) Original temporal question: Temporal questions that have no pre-processing or rewriting with Date Annotation. This variant is the most challenging data format for any given model to learn by mapping between original temporal expressions in a given question and the actual DB date values in the associated SQL query. For example:

```
What products were soldfrom 2011 to 2015?

SELECT distinct T2.PRODUCT_ID FROM SALES AS T1
JOIN SALES_DETAILS AS T2 ON T1.SALES_ID=T2.SALES_ID
WHERE T1.DATE >= '2011-01-01' AND T1.DATE <= '2015-12-31'
```

2) Full-DA question: Temporal questions that are pre-processed and re-written with Data Annotation and date values are appended at the end of questions. This variant is inspired by the mechanism that ValueNet \cite{brunner2021valuenet} used to learn values from given questions. For example:

```
what products were sold from 2011-01-01 to 2015-12-31?; 2011-01-01#date#date; 2015-12-31#date#date

SELECT distinct T2.PRODUCT_ID FROM SALES AS T1
JOIN SALES_DETAILS AS T2 ON T1.SALES_ID=T2.SALES_ID
WHERE T1.DATE >= '2011-01-01' AND T1.DATE <= '2015-12-31'
```

3) Partial-DA question: Temporal questions that are pre-processed and re-written with Data Annotation without date values appended at the end of questions. This variant can ease the learning of a given model by mapping between the normalized date values in a question and the DB values from its associated SQL query. For example:

```
what products were sold from 2011-01-01 to 2015-12-31?

SELECT distinct T2.PRODUCT_ID FROM SALES AS T1
JOIN SALES_DETAILS AS T2 ON T1.SALES_ID=T2.SALES_ID
WHERE T1.DATE >= '2011-01-01' AND T1.DATE <= '2015-12-31'
```

## Citation

TBA





