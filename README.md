# Benchmark for Structured Incremental Entity Summarization.

### Tasks ([paper](https://arxiv.org/pdf/2406.05079))
Our dataset aims to evaluate how well language models can incrementally update
entity summaries in a task called **Incremental Entity Summarization (IES)**.
The IES task is crucial for maintaining accurate and up-to-date knowledge.
It allows us to evaluate several important aspects of LLMs, including:

* Identifying and extracting attributes/values relevant to the given entity
from plain text.
* Merging partial summary of the given entity with newly added information.
* Resolving missing and conflicting information between information present
in the original table and the newly included data.

### Data (`dataset_sumie.jsonl`)
The dataset includes entity names and
categories, along with up to seven synthetically generated paragraphs. This
release provides: 1) attributes and values corresponding to each sentence in the
paragraph, and 2) IDs for distractor sentences contained in paragraphs.

We provide the dataset as .jsonl file, where each entity contains the following
information:

*   **entity name**: The entity that the summary values are generated for.
    Entity names are masked with "ENTITY"+index.
*   **entity category**: The category of the entity, i.e. hotel, restaurant,
    etc.
*   **paragraph summaries**: A list of multiple paragraphs. Each paragraph is
    augmented with sentences related to irrelevant entities.
    *   **paragraph**: generated paragraph based on the attributes and values.
        Each sentence is labeled with distinct sentence ids.
    *   **summary**: structured summary data which includes attributes, values,
        sentence ids, and evidences.
        *   **attribute**: an attribute of an entity facet (e.g. Location,
            Price)
        *   **value**: A single description of an entity’s attribute (e.g.
            accessible, expensive).
        *   **sentence ids**: sentence ids assigned to each sentence in
            paragraph.
        *   **evidences**: evidence piece of attribute and value extracted from
            the sentences in the given paragraph.
    *   **paragraph sentiment**: sentiment used for generating paragraphs (e.g.
        optimistic, neutral, pessimistic, sarcastic, humourous, analytic)
    *   **paragraph writing style**: writing style used for generating
        paragraphs (e.g. optimistic, neutral, pessimistic, sarcastic, humourous,
        analytic)
*   **distractor sentence ids**: sentence ids related to distractor entities.
    Distractor sentences are meant to challenge LLMs' ability to identify and
    ignore incorrect entity associations. All distractor sentences include
    distractor entity's name.

This is a sample data point:

```
[
    {
        "entity_name": "ENTITY0",
        "entity_category": "Computer & Video Games",
        "paragraph_summaries": [
            {
            "paragraph": [
                ["P0", "ENTITY0 offers a vast playground for gamers, with an impressive amount of gameplay content to keep them engaged for hours on end ."],
                ["P1", "HUMAN's empathy is a healing potion, allowing them to connect with others and understand their virtual and real-life struggles."],
                ["P2", "The game's flexible gameplay mechanics allow for a variety of playstyles and character builds , ensuring a unique experience for each player."],
                ["P3", "ENTITY201 has left an undeniable mark on gaming culture, fostering a passionate community that actively engages with the game and its lore."],
                ["P4", "While the game suffered from performance issues at launch , subsequent updates have significantly improved its stability and overall experience ."],
                ["P5", "HUMAN's competitiveness is a fierce rivalry, driving them to strive for victory and push their skills to the limit."],
                ["P6", "Gameplay in ENTITY0 is both immersive and engaging , drawing players into its futuristic world and keeping them hooked from start to finish."],
                ...
            ],
            "summary": [
                {
                "attribute": "Content Variety",
                "value": "Hours of gameplay content",
                "sentence_ids": ["P0", "P11"],
                "evidences": [
                    "ENTITY0 offers a vast playground for gamers, with an impressive amount of gameplay content to keep them engaged for hours on end",
                    "However, it remains a notable entry in the genre, showcasing the potential of open-world RPGs when it comes to content variety, gameplay innovation, and artistic merit"
                ]
                },
                {
                "attribute": "Gameplay",
                "value": "Multiple playstyles and skill trees",
                "sentence_ids": ["P2"],
                "evidences": ["The game's flexible gameplay mechanics allow for a variety of playstyles and character builds"]
                }
                ...
            ],
            "paragraph_sentiment": "analytic",
            "paragraph_writing_style": "user reviews by senior"
            },
            ...
        ],
        "distractor_sentences": ["P1", "P3", "P5", ...]
    }
]
```

### Evaluation

An extraction comprises three components – the attribute, its corresponding
value, and the supporting evidence. A successful extraction is one that is also
found in the set of goldens corresponding to the input paragraph.
We provide more detailed explanations below.

For automatic evaluation metrics, we use precision, recall, and F1 scores. They
are calculated as below:

Precision (P) = TP/(TP+FP) \
Recall (R) = TP/(TP+FN) \
F1 = 2*P*R / (P+R)

*   TP: true positives, where extracted attribute + values (or evidences) exist
    in the gold set.
*   FP: false positives, where extracted attribute + values do not exist in the
    gold set.
*   FN: false negatives, where gold attribute + values are not extracted in the
    predictions.


### Cite us
```
@article{hwang2024sumie,
  title={SUMIE: A Synthetic Benchmark for Incremental Entity Summarization},
  author={Hwang, Eunjeong and Zhou, Yichao and Gunel, Beliz and Wendt, James Bradley and Tata, Sandeep},
  journal={arXiv preprint arXiv:2406.05079},
  year={2024}
}
```
