# 🔎 Beyond Filenames
### Can a computer find a file when you've forgotten its name?

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR-USERNAME/beyond-filenames/blob/main/Beyond_Filenames_Benchmark.ipynb)

We all have files called `final_final_v3.docx`, `IMG_0042.png` or `bas_kar_do.pdf`.
When we need them later, we remember **what was inside**, not what we named them.
This project measures how well different search methods handle that real-life situation.

**Author:** Zara Ejaz · TRPL Solutions · October 2026

---

## Key findings

A simulated laptop with **5,624 files** (624 personal files with messy names + 5,000 unrelated documents) was searched **1,872 times** with 7 methods.

| Method | Exact words | From memory | Memory + clue* |
|---|---|---|---|
| Filename search | 8% | 0.5% | 1% |
| Keyword search (BM25) | 99% | 4% | 0% |
| Semantic – MiniLM | 99% | **79%** | 72% |
| Semantic – BGE-small | 99% | **76%** | 76% |
| Hybrid – naive blend | 99% | 22% | 10% |
| Hybrid – RRF | 100% | 21% | 10% |
| Hybrid – RRF + clue filter | 99% | 24% | **98%** |

*Correct file in the top 5 results. \*Clue = a remembered detail, e.g. "it was a PDF from March".*

1. **The word gap:** 67% of memory-style searches shared *zero* words with the file they were looking for.
2. **Meaning beats keywords from memory:** semantic search found the file ~20× more often than keyword search.
3. **The hybrid paradox:** naively mixing keyword and semantic scores dropped accuracy from 76% to ~22%. Example failure: *"the bill for fixing my computer"* returned a forum post about a legislature's "Bill Room".
4. **Clues are gold:** using one remembered clue as a filter raised results to 98%.
5. **Scales and runs locally:** ~1 ms per search on a CPU; a 9× bigger drive cost only a few points of accuracy.

![Main results](results/chart_main.png)
![Word gap](results/chart_word_gap.png)
![Scaling](results/chart_scale.png)
![Speed](results/chart_speed.png)

---

## Repository contents

| Folder / file | What it is |
|---|---|
| `Beyond_Filenames_Benchmark.ipynb` | One-click Colab notebook that runs the whole experiment |
| `recall_bench/` | Experiment code: data generation, text extraction, search methods, metrics, charts |
| `results/` | Charts, full results table, speed table, failure examples |
| `recall_app/` | **Recall**, a working local search app (Streamlit) built on the same ideas |

## Run the experiment yourself

**Easiest (no installation):** click the **Open in Colab** badge above, then `Runtime → Run all`.
It takes 20–60 minutes on the free tier (faster with `Runtime → Change runtime type → T4 GPU`).

**Locally:**
```bash
pip install -r requirements.txt
python -m recall_bench.run_benchmark
```
Settings (corpus sizes, models, screenshots on/off) are in `recall_bench/config.py`.

## How the experiment works

1. **Build a messy laptop:** 624 files in 14 everyday categories (invoices, bills, lease agreements, lecture notes, screenshots…) saved as real `.docx/.pdf/.txt/.png` files with junk names, plus up to 5,000 unrelated documents from the 20 Newsgroups dataset.
2. **Write three searches per file:** exact words, from memory, and memory + clue.
3. **Read the files** like a desktop indexer (including OCR for screenshots).
4. **Search** with 7 methods and record where the correct file ranks.
5. **Score** with Hit@1, Hit@5, Hit@10 and MRR, and measure speed.

## Limitations

This is a **simulated benchmark**, not a study of real users' files. Target files and queries were generated from templates with AI assistance, so real searches will be messier. The clue filter is idealised because dates were synthetic. Hybrid methods use default settings without tuning, models are small, and everything is in English. Results are directional evidence, not final answers.

## References

- Reimers & Gurevych (2019). *Sentence-BERT.* EMNLP.
- Thakur et al. (2021). *BEIR: A Heterogeneous Benchmark for Zero-shot Evaluation of IR Models.*
- Radford et al. (2021). *Learning Transferable Visual Models From Natural Language Supervision (CLIP).* ICML.
- Xiao et al. (2023). *C-Pack / BGE embedding models.*
- Lang (1995). *NewsWeeder* (20 Newsgroups dataset).

## License

MIT: free to use, modify and share with credit.
