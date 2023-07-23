# DIR
DIR: A Large-Scale Dialogue Rewrite Dataset for Cross-domain Conversational Text-to-SQL
This is the dataset (DIR) for the paper [DIR: A Large-Scale Dialogue Rewrite Dataset for Cross-domain Conversational Text-to-SQL](https://www.mdpi.com/2076-3417/13/4/2262). If you find it useful, please cite our work.



```
@Article{app13042262,
AUTHOR = {Li, Jieyu and Chen, Zhi and Chen, Lu and Zhu, Zichen and Li, Hanqi and Cao, Ruisheng and Yu, Kai},
TITLE = {DIR: A Large-Scale Dialogue Rewrite Dataset for Cross-Domain Conversational Text-to-SQL},
JOURNAL = {Applied Sciences},
VOLUME = {13},
YEAR = {2023},
NUMBER = {4},
ARTICLE-NUMBER = {2262},
URL = {https://www.mdpi.com/2076-3417/13/4/2262},
ISSN = {2076-3417},
ABSTRACT = {Semantic co-reference and ellipsis always lead to information deficiency when parsing natural language utterances with SQL in a multi-turn dialogue (i.e., conversational text-to-SQL task). The methodology of dividing a dialogue understanding task into dialogue utterance rewriting and language understanding is feasible to tackle this problem. To this end, we present a two-stage framework to complete conversational text-to-SQL tasks. To construct an efficient rewriting model in the first stage, we provide a large-scale dialogue rewrite dataset (DIR), which is extended from two cross-domain conversational text-to-SQL datasets, SParC and CoSQL. The dataset contains 5908 dialogues involving 160 domains. Therefore, it not only focuses on conversational text-to-SQL tasks, but is also a valuable corpus for dialogue rewrite study. In experiments, we validate the efficiency of our annotations with a popular text-to-SQL parser, RAT-SQL. The experiment results illustrate 11.81 and 27.17 QEM accuracy improvement on SParC and CoSQL, respectively, when we eliminate the semantic incomplete representations problem by directly parsing the golden rewrite utterances. The experiment results of evaluating the performance of the two-stage frameworks using different rewrite models show that the efficiency of rewrite models is important and still needs improvement. Additionally, as a new benchmark of the dialogue rewrite task, we also report the performance results of different baselines for related studies. Our dataset will be publicly available once this paper is accepted.},
DOI = {10.3390/app13042262}
}
```

## Data Content and Format
DIR contains two parts, DIR-SParC and DIR-CoSQL, collected from SParC and CoSQL respectively. Each file contains the following fields:
+ `dialog_idx`: the index of this dialogue
+ `rewrite_turn`: annotations of each turn, which contain: 
  - `db_id`: database id
  - `turn_idx`: turn index
  - `raw_utt`: the original utterance of this turn
  - `raw_utt_tok": the original utterance tokens of this turn
  - `re_utt_tok`: the rewritten utterance tokens of this turn
  - `is_rewritten`: whether the utterance is rewritten
  - `query`: the SQL query of this utterance
  - `sql`: the SQL query organized in a specialized structure, which is consistent with that in the original datasets (SParC & CoSQL)
  - `action`: a list of actions applied to rewrite this utterance, which contain:
    + `action`: action type
    + `type`: label of a more detailed category
    + `substitutionPack` or `insertBefore`: the edited position in this utterance
    + `contextPack`: the history utterance spans used in this action for rewriting
  - `useHandInput`: whether this utterance is rewritten manually. For some corner cases, we can not rewrite it completely using history utterance spans and predefined stopwords.

Following is an example:
```
{
        "dialog_idx": 108,
        "rewrite_turn": [
            {
                "db_id": "world_1",
                "turn_idx": 0,
                "raw_utt": "The countries with which government form have an average life expectancy greater than age 72?",
                "raw_utt_tok": [
                    "The",
                    "countries",
                    "with",
                    "which",
                    "government",
                    "form",
                    "have",
                    "an",
                    "average",
                    "life",
                    "expectancy",
                    "greater",
                    "than",
                    "age",
                    "72",
                    "?"
                ],
                "re_utt_tok": [
                    "The",
                    "countries",
                    "with",
                    "which",
                    "government",
                    "form",
                    "have",
                    "an",
                    "average",
                    "life",
                    "expectancy",
                    "greater",
                    "than",
                    "age",
                    "72",
                    "?"
                ],
                "is_rewritten": false,
                "query": "SELECT * FROM country GROUP BY GovernmentForm HAVING avg(LifeExpectancy)  >  72",
                "sql": {
                    "orderBy": [],
                    "from": {
                        "table_units": [
                            [
                                "table_unit",
                                2
                            ]
                        ],
                        "conds": []
                    },
                    "union": null,
                    "except": null,
                    "groupBy": [
                        [
                            0,
                            19,
                            false
                        ]
                    ],
                    "limit": null,
                    "intersect": null,
                    "where": [],
                    "having": [
                        [
                            false,
                            3,
                            [
                                0,
                                [
                                    5,
                                    15,
                                    false
                                ],
                                null
                            ],
                            72.0,
                            null
                        ]
                    ],
                    "select": [
                        false,
                        [
                            [
                                0,
                                [
                                    0,
                                    [
                                        0,
                                        0,
                                        false
                                    ],
                                    null
                                ]
                            ]
                        ]
                    ]
                },
                "action": [],
                "useHandInput": false
            },
            {
                "db_id": "world_1",
                "turn_idx": 1,
                "raw_utt": "For those, list the total population and government form name.",
                "raw_utt_tok": [
                    "For",
                    "those",
                    ",",
                    "list",
                    "the",
                    "total",
                    "population",
                    "and",
                    "government",
                    "form",
                    "name",
                    "."
                ],
                "re_utt_tok": [
                    "For",
                    "The",
                    "countries",
                    "with",
                    "which",
                    "government",
                    "form",
                    "have",
                    "an",
                    "average",
                    "life",
                    "expectancy",
                    "greater",
                    "than",
                    "age",
                    "72",
                    ",",
                    "list",
                    "the",
                    "total",
                    "population",
                    "and",
                    "government",
                    "form",
                    "name",
                    "."
                ],
                "is_rewritten": true,
                "query": "SELECT sum(Population) ,  GovernmentForm FROM country GROUP BY GovernmentForm HAVING avg(LifeExpectancy)  >  72",
                "sql": {
                    "orderBy": [],
                    "from": {
                        "table_units": [
                            [
                                "table_unit",
                                2
                            ]
                        ],
                        "conds": []
                    },
                    "union": null,
                    "except": null,
                    "groupBy": [
                        [
                            0,
                            19,
                            false
                        ]
                    ],
                    "limit": null,
                    "intersect": null,
                    "where": [],
                    "having": [
                        [
                            false,
                            3,
                            [
                                0,
                                [
                                    5,
                                    15,
                                    false
                                ],
                                null
                            ],
                            72.0,
                            null
                        ]
                    ],
                    "select": [
                        false,
                        [
                            [
                                4,
                                [
                                    0,
                                    [
                                        0,
                                        14,
                                        false
                                    ],
                                    null
                                ]
                            ],
                            [
                                0,
                                [
                                    0,
                                    [
                                        0,
                                        19,
                                        false
                                    ],
                                    null
                                ]
                            ]
                        ]
                    ]
                },
                "action": [
                    {
                        "action": "substitute",
                        "type": 4,
                        "substitutionPack": [
                            1
                        ],
                        "contextPack": [
                            {
                                "turnId": 0,
                                "tokId": 0
                            },
                            {
                                "turnId": 0,
                                "tokId": 1
                            },
                            {
                                "turnId": 0,
                                "tokId": 2
                            },
                            {
                                "turnId": 0,
                                "tokId": 3
                            },
                            {
                                "turnId": 0,
                                "tokId": 4
                            },
                            {
                                "turnId": 0,
                                "tokId": 5
                            },
                            {
                                "turnId": 0,
                                "tokId": 6
                            },
                            {
                                "turnId": 0,
                                "tokId": 7
                            },
                            {
                                "turnId": 0,
                                "tokId": 8
                            },
                            {
                                "turnId": 0,
                                "tokId": 9
                            },
                            {
                                "turnId": 0,
                                "tokId": 10
                            },
                            {
                                "turnId": 0,
                                "tokId": 11
                            },
                            {
                                "turnId": 0,
                                "tokId": 12
                            },
                            {
                                "turnId": 0,
                                "tokId": 13
                            },
                            {
                                "turnId": 0,
                                "tokId": 14
                            }
                        ]
                    }
                ],
                "useHandInput": false
            }
        ]
    },
```

