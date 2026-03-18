# 🎬 CineMood — TMDB 5000 Movie Recommender

A Streamlit app powered by the **real TMDB 5000 dataset** (4,800+ movies) that recommends top 3 films based on your mood, genre, year range, and keywords.

---

## 🚀 Quick Start

### 1. Download the dataset from Kaggle
→ https://www.kaggle.com/datasets/tmdb/tmdb-movie-metadata

Download and unzip. You get:
- `tmdb_5000_movies.csv`
- `tmdb_5000_credits.csv`

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Run the app
```bash
streamlit run app.py
```

### 4. Upload your CSVs inside the app
The UI has two upload buttons — drop in both files.

---

## 🧠 How It Works

| Step | Detail |
|------|--------|
| **JSON Parsing** | `genres`, `keywords`, `cast`, `crew` columns are JSON strings → extracted into clean lists |
| **Mood Detection** | Scans input text for 16 mood keywords |
| **Mood Mapping** | Maps mood → TMDB genre names (e.g. `sad` → `["comedy","romance","family"]`) |
| **Query Building** | Mood features + user genre + user keywords → single query string |
| **TF-IDF** | Vectorizes `genres + keywords + overview + cast` per movie (20k vocab) |
| **Cosine Similarity** | Scores all movies against query |
| **Year Filter** | Optional slider filter on `release_date` |
| **Weighted Rating** | IMDB-style formula: `(v/(v+m))×R + (m/(v+m))×C` (penalises low vote counts) |
| **Final Ranking** | Top 50 by similarity → top 3 by weighted rating |

---

## 🎭 Mood Map

| Mood | Mapped TMDB Genres |
|------|--------------------|
| sad / unhappy / crying | Comedy, Romance, Family |
| bored / dull | Thriller, Action, Crime |
| stressed / anxious / tired | Animation, Family, Comedy |
| excited / happy | Adventure, Science Fiction, Action |
| romantic / lonely | Romance, Drama |
| angry | Thriller, Action, Crime |
| curious | Science Fiction, Mystery, Thriller |
| nostalgic | Drama, History, Romance |
| scared | Comedy, Animation, Family |

**Default** (no mood detected): Thriller + Action

---

## 📦 Project Files

```
movie_recommender_tmdb/
├── app.py           # Main Streamlit app
├── requirements.txt # Dependencies
└── README.md
```

The app is **upload-based** — no hardcoded paths. Just run and upload via the UI.

---

## ✨ Output per Movie

- Title, Genres, Release Year
- ⭐ TMDB Rating + 🏆 Weighted Rating  
- Top 3 cast members + Director
- Movie overview (truncated)
- Natural language explanation of why it was recommended
