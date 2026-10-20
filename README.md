# Knowledge poisoning in a RAG system: initial implementation

This repository holds my first implementation of a retrieval augmented generation (RAG) system attacked by knowledge poisoning. It follows the task given by my supervisor: build a complete RAG system with a vector database, add poisoned data so that the poisoning shows in the behaviour, check the behaviour, remove the poison and check again, then add the poison back and check once more.

**Author:** Rameesha, MS Data Science, FAST-NUCES Islamabad. **Supervisor:** Dr. Anwar Shah.

## Setup

| Part | Choice |
|---|---|
| Data | 100 questions from the SQuAD validation set |
| Knowledge base | 500 paragraphs (the 100 answer paragraphs and 400 unrelated ones) |
| Vector database | Chroma, cosine distance |
| Retriever | sentence-transformers/multi-qa-MiniLM-L6-cos-v1, top 5 passages |
| Generator | Qwen/Qwen2.5-3B-Instruct on a Colab T4 GPU |
| Attack | Black box attack of PoisonedRAG (Zou et al., USENIX Security 2025, arXiv 2402.07867), 5 poisoned passages per question |
| Seed | 42 |

## Results

| Stage | Accuracy | Attack success | Poisoned passages in top 5 |
|---|---|---|---|
| Clean | 75% | 0% | 0 |
| Poisoned | 0% | 93% | 4.97 |
| Poison removed | 75% | 0% | 0 |
| Poison added again | 0% | 93% | 4.97 |

After removal the answers were 100% identical to the clean run, and after adding the poison again they were 100% identical to the first poisoned run, so the change in behaviour comes from the poisoned passages alone.

| Poisoned passages per question | 0 | 1 | 2 | 3 | 5 |
|---|---|---|---|---|---|
| Accuracy | 75% | 27% | 15% | 11% | 0% |
| Attack success | 0% | 61% | 71% | 80% | 93% |

70 of the 100 questions changed from a correct answer to the attacker's answer.

## Limits

For 14 questions the model gave no usable wrong answer, so the answer of another question was used as the target; attack success is 93% on all 100 questions and 98.8% on the other 86. A bootstrap over the 100 questions puts the attack success between 88% and 97%. This is a single run with one model and a small knowledge base, and correctness is checked by text matching.

## Files

| File | Content |
|---|---|
| `RAG_Poisoning_Initial_Implementation.ipynb` | The Colab notebook with all outputs |
| `rag_poisoning_results.zip` | Graphs (four stages, retrieval of poison, amount of poison), tables and all answers as CSV |

## How to run

Open the notebook in Google Colab, choose a T4 GPU under Runtime, and run all cells. A run takes about 20 to 30 minutes.
