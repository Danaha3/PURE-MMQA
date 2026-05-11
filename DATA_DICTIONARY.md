
## Source Documents

The dataset was constructed from a controlled subset of 10 Software Requirements Specification (SRS) or Functional Requirements Specification (FRS) documents selected from the PURE requirements corpus. The subset includes both Word and PDF documents and was selected for methodological feasibility: documents had to be SRS/FRS-like, contain usable requirements content, avoid heavy masking or placeholder text, and fit within a single generation context.

The 10 source documents are:

| ID | Format | Document | Indicative domain |
|---|---|---|---|
| 1 | Word | 2004 -- Philips | Consumer/product requirements |
| 2 | Word | 2009 -- Library | Library/information system |
| 3 | Word | 2004 -- IJIS | Justice/information integration |
| 4 | Word | 2001 -- Libra | Information system |
| 5 | Word | 2007 -- E-Store | E-commerce |
| 6 | PDF | 2003 -- QHeadache | Healthcare/medical support |
| 7 | PDF | 2003 -- AgentMom | Personal assistant/agent |
| 8 | PDF | 2005 -- Grid 3D | 3D/grid visualisation |
| 9 | PDF | 2007 -- Get Real 0.2 | Application requirements |
| 10 | PDF | 2007 -- Central Trading System | Trading/financial system |

The source documents originate from the PURE requirements dataset. Users should cite the original PURE dataset when using these source documents.


## Overview
This data dictionary describes the five Excel files used in the PURE-MMQA dataset: `EVL.xlsx`, `EXE.xlsx`, `GOV.xlsx` ,`DIS.xlsx`  and `QA Dataset.xlsx`. Each row represents one retained question-answer pair from the PURE-MMQA dataset construction and filtering pipeline.

The main file, `QA Dataset.xlsx`., is the consolidated version of the dataset. It combines 306 QA pairs from the four task-specific Excel files:
EVL.xlsx — 59 QA instances
EXE.xlsx — 98 QA instances
GOV.xlsx — 57 QA instances
DIS.xlsx — 92 QA instances
Total = 306

The consolidated `QA Dataset.xlsx` contains 9 fields:
ID, Question, Answer, Document name, Task, type, Difficulty, section number, and page reference.

The four task-specific Excel files contain the full construction and evaluation metadata. Each task file contains 92 fields, including the source SRS document, task metadata, generated questions and answers, provenance fields such as page references, section hints, context, and images used, rubric-based LLM-judge scores and justifications, and difficulty relabeling outputs.

Overall, `QA Dataset.xlsx` provides a simplified release-ready version of the dataset, while the four task-specific files preserve the detailed pipeline outputs needed for evaluation, and reproducibility.


## Row Unit details for the four task-specific Excel

One row = one QA item generated from a source Software Requirements Specification (SRS) document and evaluated by multiple LLM judges.

## Core Dataset Fields

| Field | Type | Description | Example / Allowed values |
|---|---|---|---|
| `model_source` | Text | Generation model or source label used to create the QA item. | `GPT` |
| `pdf_file` | Text | Source SRS/FRS document filename. | e.g `2009 - library.pdf` |
| `task` | Text | High-level task code for the QA item. | Ie.g `DIS` |
| `task_code` | Text | Canonical task code. | e.g: `DIS` |
| `task_name` | Text | Full name of the task category. | `e.g Requirements Discovery` |
| `subtask_name` | Categorical text | Specific subtask within the task category. | e.g `Search`; `Stakeholder Analysis` |
| `question_number` | Integer | Sequence number of the generated question within the document/task/subtask grouping. | e.g `1`, `2`, `3`, ... |
| `question` | Text | Generated natural-language question. | Free text |
| `modality` | Categorical text | Evidence modality used by the QA item. | e.g `text_only`; `multimodal` |
| `difficulty` | Categorical text | Original generated difficulty label before majority relabeling. | e.g `Easy`; `Hard`|
| `evidence_type` | Categorical text | Whether the evidence is directly stated or requires inference from the SRS evidence. | e.g `Explicit`; `Implicit` |
| `section_hint` | Text | Section title, section number, appendix name, or nearby textual hint used as provenance. | `Appendix B – Fine-Grained Requirements` |
| `page_reference` | Text | Page number(s) where the evidence appears. | e.g `Page 8`; `Page 8; Page 16` |
| `images_used` | Text / Empty | Image or figure filename(s) used for multimodal items. Empty for text-only items. | `None` / image filename(s) |
| `context` | Long text | Supporting SRS text supplied to generation/evaluation, usually with page delimiters. | Long text with page markers |
| `direct_answer` | Text | Short answer grounded directly in source evidence. | Free text |
| `generated_answer` | Text | Generated answer evaluated by the judge models. | Free text |
| `voted_difficulty` | Categorical text | Final difficulty label after majority-vote relabeling logic. | `Easy`; `Medium`; `Hard` |
| `voted_difficulty_score` | Integer | Majority difficulty-alignment score used during relabeling; follows the 0--2 rubric score scale. | e.g `0`, `1`, `2` |

## Question-Level Evaluation Criteria

Question-level criteria evaluate the generated question itself.

| Criterion prefix | Description |
|---|---|
| `q_clarity` | Evaluates whether the question is clear, concise, grammatical, and unambiguous. |
| `q_passage_relevance` | Evaluates whether the question is topically aligned with the cited passage and/or multimodal evidence. |
| `q_answerability` | Evaluates whether the question can be answered from the provided SRS evidence. |
| `task_relevance` | Evaluates whether the question fits the assigned task/subtask category. |

For each criterion, the file contains one score and one reason per judge:

```text
<criterion>_score_<judge>
<criterion>_reason_<judge>
```
## Answer-Level Evaluation Criteria

Answer-level criteria evaluate both the `direct_answer` and the `generated_answer`.

| Criterion prefix | Answer evaluated | Description |
|---|---|---|
| `direct_a_Clarity` | `direct_answer` | Evaluates clarity, fluency, grammar, and understandability of the direct answer. |
| `gen_a_Clarity` | `generated_answer` | Evaluates clarity, fluency, grammar, and understandability of the generated answer. |
| `direct_a_faithfulness` | `direct_answer` | Evaluates whether the direct answer is grounded in the cited SRS evidence. |
| `gen_a_faithfulness` | `generated_answer` | Evaluates whether the generated answer is grounded in the cited SRS evidence. |
| `direct_a_correctness` | `direct_answer` | Evaluates factual correctness and completeness of the direct answer. |
| `gen_a_correctness` | `generated_answer` | Evaluates factual correctness and completeness of the generated answer. |
| `direct_a_difficulty` | `direct_answer` | Evaluates whether the assigned difficulty label fits the direct-answer task. |
| `gen_a_difficulty` | `generated_answer` | Evaluates whether the assigned difficulty label fits the generated-answer task. |

For each answer-level criterion, the file contains one score and one reason per judge:

```text
<answer_criterion>_score_<judge>
<answer_criterion>_reason_<judge>
```

## The judge suffixes are:

| Judge suffix | Judge model |
|---|---|
| `gpt-5.4` | GPT-family judge |
| `gemini-3-flash-preview` | Gemini judge |
| `deepseek-reasoner` | DeepSeek Reasoner judge |

Example 1:
```text
q_clarity_score_gpt-5.4
q_clarity_reason_gpt-5.4
```

Example 2:
```text
gen_a_faithfulness_score_deepseek-reasoner
gen_a_faithfulness_reason_deepseek-reasoner
```

## Notes for Reuse

- The `context` field may contain long source passages from the SRS documents. Before public redistribution, verify that the underlying source-document license permits including full or extended text passages.
- Empty `images_used` values generally indicate text-only QA items.
- `modality = multimodal` indicates that at least one figure/image was used or referenced in the QA item.
- The three judge models are evaluated independently; reason fields should not be treated as gold human annotations.
- The current file is a curated model-judged dataset slice. Human validation may be added later for expert calibration.
