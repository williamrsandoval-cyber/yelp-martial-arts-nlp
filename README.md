[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/williamrsandoval-cyber/yelp-martial-arts-nlp/blob/main/ProjectFinal_Sandoval_William_GitHub.ipynb)


# What Drives Enrollment Decisions in Martial Arts Schools?

> A natural language analysis of **3,212 Yelp reviews** across **265 martial arts businesses**, comparing six NLP methods to identify the language patterns that distinguish satisfied from dissatisfied customers — with directly actionable findings for school owners.

**Author:** William Sandoval
**Program:** MS Applied Information Behavior, Arizona State University
**Course:** Final Project — NLP for Business Applications
**Owner:** [Patriot Martial Arts Academy](https://patriotmartialartsacademy.com) (Brazilian Jiu-Jitsu + Machida Karate)

---

## The Business Problem

Prospective martial arts students — and the parents enrolling kids — overwhelmingly read Yelp before they ever walk into a dojo. For school owners, those reviews are simultaneously the front door of the business and a blind spot: owners read reviews one at a time and miss the patterns that repeat across hundreds.

This project asks: **what factors in Yelp reviews influence a customer's decision to enroll in a martial arts school?** The goal is not just to describe what reviewers say, but to surface themes that can drive real changes in marketing, operations, and retention.

---

## Dataset

- **Source:** Yelp Open Dataset (filtered locally before upload)
- **Filter:** Businesses categorized under martial arts
- **Final corpus:** 3,212 reviews across 265 businesses
- **Average review length:** 122 words
- **Average star rating:** 4.22 stars
- **Cleaning:** dropped 3-star reviews (mixed/neutral) and null text; binary label assigned (1-2★ → 0, 4-5★ → 1)
- **Class balance:** 82% positive, 18% negative — analytical implication addressed via stratified train/test split and per-class metrics

---

## Methods

Six NLP methods were applied, deliberately spanning the full course curriculum from classical lexicon-based approaches to modern LLM prompting:

| # | Method | Type | Source |
|---|--------|------|--------|
| 1 | VADER Sentiment | Lexicon-based polarity | Classical baseline |
| 2 | Llama 3.2 (Zero-Shot + Few-Shot) | LLM prompting via Ollama | Modern alternative |
| 3 | LDA Topic Modeling | Probabilistic topic model | Classical baseline |
| 4 | BERTopic | Sentence-transformer + UMAP + HDBSCAN | Modern alternative |
| 5 | TF-IDF + Linear SVM | Classical ML classifier | Classical baseline |
| 6 | GRU + Frozen GloVe | Deep learning classifier | Modern alternative |

Each pair (sentiment, topics, classification) provides a classical-vs-modern comparison.

---

## Headline Results

**F1 score on the negative class** (the metric that matters under class imbalance):

| Method | F1 (negative) | Eval Set |
|--------|--------------:|----------|
| **Llama 3.2 Few-Shot** | **0.95** | balanced 200 |
| TF-IDF + Linear SVM | 0.89 | stratified 643 |
| GRU + Frozen GloVe | 0.85 | stratified 643 |
| Llama 3.2 Zero-Shot | 0.84 | balanced 200 |
| VADER (200-sample) | 0.77 | balanced 200 |
| VADER (full data) | 0.68 | all 3,212 |

The TF-IDF + Linear SVM also achieved **96.3% overall accuracy** on a 643-review held-out test set with 82% recall on the negative class — operationally usable as an always-on review classifier.

---

## Key Findings

1. **Coaching is the product, not the curriculum.**
   Both LDA and BERTopic surface a dominant theme around instructors, coaching, and welcoming atmosphere. The SVM's top positive coefficients (great, love, helpful, amazing, welcoming) almost all describe people and experience. Marketing should lead with the coach.

2. **Operational friction kills more memberships than bad classes do.**
   The SVM's top negative coefficients (rude, told, cancel, money, contract, business, owner) are almost entirely about operations, not training. BERTopic's `topics_per_class` confirms 1-2★ reviews cluster on contracts, billing, and front-desk experience.

3. **A two-stage pipeline is the right operational design.**
   The SVM is fast, deterministic, and interpretable — ideal for triage of incoming reviews. Llama 3.2 with few-shot prompting is the right tool for context-aware extraction on flagged reviews (extracting *what specifically* the reviewer disliked).

---

## Repository Structure

```
.
├── README.md                                  # This file
├── requirements.txt                           # Python dependencies
├── LICENSE                                    # MIT
├── .gitignore                                 # Standard Python + Jupyter
├── ProjectFinal_Sandoval_William_GitHub.ipynb     # Main notebook
├── presentation/
│   └── Sandoval_Final_Presentation_v2.pptx    # Slide deck
└── data/
    └── README.md                              # Instructions for obtaining the Yelp dataset
```

---

## How to Run

### Recommended environment
- **Google Colab** with **T4 GPU** runtime (free tier sufficient)
- End-to-end runtime: ~30-45 minutes

### Quick start

1. Clone this repository or download the notebook directly:
   ```bash
   git clone https://github.com/<your-username>/yelp-martial-arts-nlp.git
   cd yelp-martial-arts-nlp
   ```

2. Obtain the Yelp dataset (see `data/README.md` for instructions). Filter locally and place `business_filtered.csv` and `reviews_filtered.csv` in the working directory.

3. Open `ProjectFinal_Sandoval_William_GitHub.ipynb` in Colab (`File → Open notebook → GitHub`).

4. Set runtime to T4 GPU (`Runtime → Change runtime type → T4 GPU`).

5. Run cells top to bottom. The Llama section (Section 8) requires the Ollama install cells to run first; this takes ~5 minutes.

### Local installation
```bash
pip install -r requirements.txt
jupyter notebook ProjectFinal_Sandoval_William_GitHub.ipynb
```

Note that running Llama 3.2 locally requires a separate [Ollama](https://ollama.com) installation.

---

## Limitations

- **Self-selection bias.** Yelp reviewers are voluntary participants and skew toward engaged customers. Findings are directional, not causal.
- **Eval-set asymmetry.** Llama and VADER were scored on a balanced 200-review sample for runtime tractability; SVM and GRU were scored on the full 643-review stratified test set. The method ordering is robust across both slices but they are not perfectly comparable.
- **Topic overlap.** LDA topics overlap because vocabulary overlaps. BERTopic provides cleaner separation but introduces an "outlier" topic (-1) that absorbs unclustered reviews.
- **English-only.** The pipeline assumes English-language reviews; international markets would require multilingual embeddings.

---

## Future Work

- Apply the same pipeline to **Google reviews** (higher volume than Yelp for most martial arts schools).
- Extend the LLM prompts to **aspect-level extraction** (instructor / contract / facility / pricing) instead of binary sentiment.
- Build a **review triage dashboard** that auto-scores incoming reviews and flags negatives for same-day owner response.
- Validate findings with a **field deployment** at Patriot Martial Arts Academy on its own incoming review feed.

---

## Course Context

This project demonstrates techniques from the full course progression:

| Lab | Topic | Used in |
|-----|-------|---------|
| LA1 | Text preprocessing fundamentals | Section 6 |
| LA2 | Classical NLP (VADER, LDA, TF-IDF, SVM) | Sections 7, 9, 11 |
| LA3 | Word embeddings | (foundational for LA4) |
| LA4 | Deep learning for text (GRU + GloVe) | Section 12 |
| LA5 | BERTopic and transformer-based topic modeling | Section 10 |
| LA6 | LLM prompting (Ollama, zero-shot, few-shot) | Section 8 |

---

## License

MIT License — see `LICENSE` for details. The Yelp dataset itself is governed by the [Yelp Dataset License](https://www.yelp.com/dataset/download) and is not redistributed in this repository.

---

## Acknowledgments

Built as part of the MS Applied Information Behavior program at Arizona State University. The personal motivation for this project comes from the author's experience as the owner of Patriot Martial Arts Academy.
