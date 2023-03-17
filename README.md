# Dataset TempQ4NLIDB

This dataset **TempQ4NLIDB** consists of the following contents:

1) Two accompanied databases Warehouse/Sales (WH) and Human Resource (HR) compatible with SQLite.

2) Temporal questions and associated SQL queries for each database. There are 3 variants: Original temporal questions, Full-DA questions, and Partial-DA questions.

3) Non-temporal questions and associated SQL queries for each database.

4) Mixed of Full-DA temporal and non-temporal questions for each database.

## Data format:

Every data point consists of one natural language question and one associated SQL query regardless if it is a temporal or a non-temporal question. We use JSON data format which is compatible with Spider.

### Temporal questions

There are three variants of temporal questions:

**1) Original temporal question:** Temporal questions that have no pre-processing or rewriting with Date Annotation. This variant is the most challenging data format for any given model to learn by mapping between original temporal expressions in a given question and the actual DB date values in the associated SQL query. For example:

<pre><code>
{
  "question": "what are the names of employees who work in Sales department and were hired before <i>Christmas 2015</i>",
  "query": "select distinct EMPLOYEE.EMPNAME from EMPLOYEE where EMPLOYEE.DPTNAME  =  'Sales' and <b>EMPLOYEE.HIREDATE  <  '2015-12-25'</b>",
  "query_tok": [
   "select",
   "distinct",
   "EMPLOYEE.EMPNAME",
   "from",
   "EMPLOYEE",
   "where",
   "EMPLOYEE.DPTNAME",
   "=",
   "Sales",
   "and",
   "EMPLOYEE.HIREDATE",
   "<",
   "2015-12-25"
  ],
  "db_id": "HR",
  "question_toks": [
   "what",
   "are",
   "name",
   "of",
   "employee",
   "who",
   "work",
   "in",
   "sale",
   "department",
   "and",
   "were",
   "hired",
   "before",
   "christmas",
   "year"
  ],
  "names": [
   "*",
   "empno",
   "empname",
   "mgrname",
   "birthdate",
   "hiredate",
   "leavedate",
   "salary",
   "bonus",
   "dptname"
  ],
  "table_names": [
   "employee"
  ],
  "col_set": [
   "*",
   "empno",
   "empname",
   "mgrname",
   "birthdate",
   "hiredate",
   "leavedate",
   "salary",
   "bonus",
   "dptname"
  ],
  "col_table": [
   -1,
   0,
   0,
   0,
   0,
   0,
   0,
   0,
   0,
   0
  ],
  "keys": {},
  "origin_question_toks": [
   "what",
   "are",
   "the",
   "names",
   "of",
   "employees",
   "who",
   "work",
   "in",
   "Sales",
   "department",
   "and",
   "were",
   "hired",
   "before",
   "Christmas",
   "2015"
  ],
  "question_arg": [
   [
    "what"
   ],
   [
    "are"
   ],
   [
    "name"
   ],
   [
    "of"
   ],
   [
    "employee"
   ],
   [
    "who"
   ],
   [
    "work"
   ],
   [
    "in"
   ],
   [
    "sale"
   ],
   [
    "department"
   ],
   [
    "and"
   ],
   [
    "were"
   ],
   [
    "hired"
   ],
   [
    "before"
   ],
   [
    "christmas"
   ],
   [
    "year"
   ]
  ],
  "question_arg_type": [
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "table"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ]
  ],
  "nltk_pos": [
   [
    "what",
    "WDT"
   ],
   [
    "are",
    "VBP"
   ],
   [
    "name",
    "NN"
   ],
   [
    "of",
    "IN"
   ],
   [
    "employee",
    "NN"
   ],
   [
    "who",
    "WP"
   ],
   [
    "work",
    "VBP"
   ],
   [
    "in",
    "IN"
   ],
   [
    "sale",
    "NN"
   ],
   [
    "department",
    "NN"
   ],
   [
    "and",
    "CC"
   ],
   [
    "were",
    "VBD"
   ],
   [
    "hired",
    "VBN"
   ],
   [
    "before",
    "IN"
   ],
   [
    "christmas",
    "NN"
   ],
   [
    "2015",
    "CD"
   ]
  ],
  "ner_extracted_values": [],
  "values": [
   "Sales",
   "2015-12-25"
  ],
  "ner_extracted_values_processed": [
   "2015"
  ],
  "all_values_found": "True",
  "sql": {
   "from": {
    "table_units": [
     [
      "table_unit",
      0
     ]
    ],
    "conds": []
   },
   "select": [
    true,
    [
     [
      0,
      [
       0,
       [
        0,
        2,
        false
       ],
       null
      ]
     ]
    ]
   ],
   "where": [
    [
     false,
     2,
     [
      0,
      [
       0,
       9,
       false
      ],
      null
     ],
     "Sales",
     null
    ],
    "and",
    [
     false,
     4,
     [
      0,
      [
       0,
       5,
       false
      ],
      null
     ],
     "2015-12-25",
     null
    ]
   ],
   "groupBy": [],
   "having": [],
   "orderBy": [],
   "limit": null,
   "intersect": null,
   "union": null,
   "except": null
  }
 }
</pre></code>

**2) Full-DA question:** Temporal questions that are pre-processed and re-written with Data Annotation (DA) and date values are appended at the end of questions. This variant is inspired by the mechanism that ValueNet \cite{brunner2021valuenet} used to learn values from given questions. For example:

<pre><code>
{
  "question": "what are the names of employees who work in Sales department and were hired before <b>2015-12-25</b>; <b>2015-12-25#date#date</b>",
  "query": "select distinct EMPLOYEE.EMPNAME from EMPLOYEE where EMPLOYEE.DPTNAME  =  'Sales' and <b>EMPLOYEE.HIREDATE  <  '2015-12-25'</b>",
  "query_tok": [
   "select",
   "distinct",
   "EMPLOYEE.EMPNAME",
   "from",
   "EMPLOYEE",
   "where",
   "EMPLOYEE.DPTNAME",
   "=",
   "Sales",
   "and",
   "EMPLOYEE.HIREDATE",
   "<",
   "2015-12-25"
  ],
  "db_id": "HR",
  "question_toks": [
   "what",
   "are",
   "name",
   "of",
   "employee",
   "who",
   "work",
   "in",
   "sale",
   "department",
   "and",
   "were",
   "hired",
   "before",
   "year",
   "-",
   "12",
   "-",
   "25",
   ";",
   "year",
   "-",
   "12",
   "-",
   "25#date#date"
  ],
  "names": [
   "*",
   "empno",
   "empname",
   "mgrname",
   "birthdate",
   "hiredate",
   "leavedate",
   "salary",
   "bonus",
   "dptname"
  ],
  "table_names": [
   "employee"
  ],
  "col_set": [
   "*",
   "empno",
   "empname",
   "mgrname",
   "birthdate",
   "hiredate",
   "leavedate",
   "salary",
   "bonus",
   "dptname"
  ],
  "col_table": [
   -1,
   0,
   0,
   0,
   0,
   0,
   0,
   0,
   0,
   0
  ],
  "keys": {},
  "origin_question_toks": [
   "what",
   "are",
   "the",
   "names",
   "of",
   "employees",
   "who",
   "work",
   "in",
   "Sales",
   "department",
   "and",
   "were",
   "hired",
   "before",
   "2015",
   "-",
   "12",
   "-",
   "25",
   ";",
   "2015",
   "-",
   "12",
   "-",
   "25#date#date"
  ],
  "question_arg": [
   [
    "what"
   ],
   [
    "are"
   ],
   [
    "name"
   ],
   [
    "of"
   ],
   [
    "employee"
   ],
   [
    "who"
   ],
   [
    "work"
   ],
   [
    "in"
   ],
   [
    "sale"
   ],
   [
    "department"
   ],
   [
    "and"
   ],
   [
    "were"
   ],
   [
    "hired"
   ],
   [
    "before"
   ],
   [
    "year"
   ],
   [
    "-"
   ],
   [
    "12"
   ],
   [
    "-"
   ],
   [
    "25"
   ],
   [
    ";"
   ],
   [
    "year"
   ],
   [
    "-"
   ],
   [
    "12"
   ],
   [
    "-"
   ],
   [
    "25#date#date"
   ]
  ],
  "question_arg_type": [
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "table"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "value"
   ],
   [
    "NONE"
   ],
   [
    "value"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "value"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ]
  ],
  "nltk_pos": [
   [
    "what",
    "WDT"
   ],
   [
    "are",
    "VBP"
   ],
   [
    "name",
    "NN"
   ],
   [
    "of",
    "IN"
   ],
   [
    "employee",
    "NN"
   ],
   [
    "who",
    "WP"
   ],
   [
    "work",
    "VBP"
   ],
   [
    "in",
    "IN"
   ],
   [
    "sale",
    "NN"
   ],
   [
    "department",
    "NN"
   ],
   [
    "and",
    "CC"
   ],
   [
    "were",
    "VBD"
   ],
   [
    "hired",
    "VBN"
   ],
   [
    "before",
    "IN"
   ],
   [
    "2015",
    "CD"
   ],
   [
    "-",
    ":"
   ],
   [
    "12",
    "CD"
   ],
   [
    "-",
    ":"
   ],
   [
    "25",
    "CD"
   ],
   [
    ";",
    ":"
   ],
   [
    "2015",
    "CD"
   ],
   [
    "-",
    ":"
   ],
   [
    "12",
    "CD"
   ],
   [
    "-",
    ":"
   ],
   [
    "25#date#date",
    "CD"
   ]
  ],
  "ner_extracted_values": [],
  "values": [
   "Sales",
   "2015-12-25"
  ],
  "ner_extracted_values_processed": [],
  "all_values_found": "True",
  "sql": {
   "from": {
    "table_units": [
     [
      "table_unit",
      0
     ]
    ],
    "conds": []
   },
   "select": [
    true,
    [
     [
      0,
      [
       0,
       [
        0,
        2,
        false
       ],
       null
      ]
     ]
    ]
   ],
   "where": [
    [
     false,
     2,
     [
      0,
      [
       0,
       9,
       false
      ],
      null
     ],
     "Sales",
     null
    ],
    "and",
    [
     false,
     4,
     [
      0,
      [
       0,
       5,
       false
      ],
      null
     ],
     "2015-12-25",
     null
    ]
   ],
   "groupBy": [],
   "having": [],
   "orderBy": [],
   "limit": null,
   "intersect": null,
   "union": null,
   "except": null
  }
 }
</pre></code>

**3) Partial-DA question:** Temporal questions that are pre-processed and re-written with Data Annotation (DA) without date values appended at the end of questions. This variant can ease the learning of a given model by mapping between the normalized date values in a question and the DB values from its associated SQL query. For example:

<pre><code>
{
  "question": "what are the names of employees who work in Sales department and were hired before <b>2015-12-25</b>",
  "query": "select distinct EMPLOYEE.EMPNAME from EMPLOYEE where EMPLOYEE.DPTNAME  =  'Sales' and <b>EMPLOYEE.HIREDATE  <  '2015-12-25'</b>",
  "query_tok": [
   "select",
   "distinct",
   "EMPLOYEE.EMPNAME",
   "from",
   "EMPLOYEE",
   "where",
   "EMPLOYEE.DPTNAME",
   "=",
   "Sales",
   "and",
   "EMPLOYEE.HIREDATE",
   "<",
   "2015-12-25"
  ],
  "db_id": "HR",
  "question_toks": [
   "what",
   "are",
   "name",
   "of",
   "employee",
   "who",
   "work",
   "in",
   "sale",
   "department",
   "and",
   "were",
   "hired",
   "before",
   "year",
   "-",
   "12",
   "-",
   "25"
  ],
  "names": [
   "*",
   "empno",
   "empname",
   "mgrname",
   "birthdate",
   "hiredate",
   "leavedate",
   "salary",
   "bonus",
   "dptname"
  ],
  "table_names": [
   "employee"
  ],
  "col_set": [
   "*",
   "empno",
   "empname",
   "mgrname",
   "birthdate",
   "hiredate",
   "leavedate",
   "salary",
   "bonus",
   "dptname"
  ],
  "col_table": [
   -1,
   0,
   0,
   0,
   0,
   0,
   0,
   0,
   0,
   0
  ],
  "keys": {},
  "origin_question_toks": [
   "what",
   "are",
   "the",
   "names",
   "of",
   "employees",
   "who",
   "work",
   "in",
   "Sales",
   "department",
   "and",
   "were",
   "hired",
   "before",
   "2015",
   "-",
   "12",
   "-",
   "25"
  ],
  "question_arg": [
   [
    "what"
   ],
   [
    "are"
   ],
   [
    "name"
   ],
   [
    "of"
   ],
   [
    "employee"
   ],
   [
    "who"
   ],
   [
    "work"
   ],
   [
    "in"
   ],
   [
    "sale"
   ],
   [
    "department"
   ],
   [
    "and"
   ],
   [
    "were"
   ],
   [
    "hired"
   ],
   [
    "before"
   ],
   [
    "year"
   ],
   [
    "-"
   ],
   [
    "12"
   ],
   [
    "-"
   ],
   [
    "25"
   ]
  ],
  "question_arg_type": [
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "table"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "NONE"
   ],
   [
    "value"
   ],
   [
    "NONE"
   ],
   [
    "value"
   ]
  ],
  "nltk_pos": [
   [
    "what",
    "WDT"
   ],
   [
    "are",
    "VBP"
   ],
   [
    "name",
    "NN"
   ],
   [
    "of",
    "IN"
   ],
   [
    "employee",
    "NN"
   ],
   [
    "who",
    "WP"
   ],
   [
    "work",
    "VBP"
   ],
   [
    "in",
    "IN"
   ],
   [
    "sale",
    "NN"
   ],
   [
    "department",
    "NN"
   ],
   [
    "and",
    "CC"
   ],
   [
    "were",
    "VBD"
   ],
   [
    "hired",
    "VBN"
   ],
   [
    "before",
    "IN"
   ],
   [
    "2015",
    "CD"
   ],
   [
    "-",
    ":"
   ],
   [
    "12",
    "CD"
   ],
   [
    "-",
    ":"
   ],
   [
    "25",
    "CD"
   ]
  ],
  "ner_extracted_values": [],
  "values": [
   "Sales",
   "2015-12-25"
  ],
  "ner_extracted_values_processed": [],
  "all_values_found": "True",
  "sql": {
   "from": {
    "table_units": [
     [
      "table_unit",
      0
     ]
    ],
    "conds": []
   },
   "select": [
    true,
    [
     [
      0,
      [
       0,
       [
        0,
        2,
        false
       ],
       null
      ]
     ]
    ]
   ],
   "where": [
    [
     false,
     2,
     [
      0,
      [
       0,
       9,
       false
      ],
      null
     ],
     "Sales",
     null
    ],
    "and",
    [
     false,
     4,
     [
      0,
      [
       0,
       5,
       false
      ],
      null
     ],
     "2015-12-25",
     null
    ]
   ],
   "groupBy": [],
   "having": [],
   "orderBy": [],
   "limit": null,
   "intersect": null,
   "union": null,
   "except": null
  }
 }
</pre></code>

## Citation

TBA





