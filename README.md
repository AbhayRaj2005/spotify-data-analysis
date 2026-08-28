# 🎵 Spotify Track Genre Analysis

A data analysis and machine learning project based on a Spotify tracks dataset containing 119,000 tracks and 21 columns.
The project focuses on understanding Spotify track data through data cleaning, preprocessing, exploratory data analysis, feature preparation, and genre-related analysis.

## 📌 Project Overview

This project follows a practical data science workflow:

1. **Data Loading** – Load and inspect the Spotify tracks dataset.
2. **Data Cleaning** – Identify and handle missing and inconsistent values.
3. **Data Preprocessing** – Convert relevant columns into suitable numerical formats.
4. **Feature Analysis** – Examine Spotify audio features and categorical variables.
5. **Exploratory Data Analysis** – Explore distributions, relationships, and genre information.
6. **Genre Preparation** – Prepare `track_genre` as a categorical target for modeling.
7. **Model Building** – Train a Random Forest Classifier to predict track genre from audio features.
8. **Recommendation System** – Generate similar-track playlists using cosine similarity.

## 📊 Dataset

The dataset (`spotify.csv` / `spotify.csv.xlsx`) contains:

- **119,000 tracks**
- **21 columns**
- **114 unique track genres**

Important columns include:

| Column | Description |
|---|---|
| `track_id` | Unique track identifier |
| `artists` | Artist name(s) |
| `album_name` | Album name |
| `track_name` | Track name |
| `popularity` | Track popularity score |
| `duration_ms` | Track duration |
| `explicit` | Explicit-content indicator |
| `danceability` | Danceability score |
| `energy` | Energy score |
| `loudness` | Loudness level |
| `speechiness` | Presence of spoken words |
| `acousticness` | Acoustic confidence |
| `instrumentalness` | Instrumental content |
| `liveness` | Presence of live audience |
| `valence` | Musical positivity |
| `tempo` | Tempo in BPM |
| `key` | Musical key |
| `mode` | Major/minor mode |
| `time_signature` | Estimated time signature |
| `track_genre` | Track genre |

> ⚠️ The dataset file (`spotify.csv` or `spotify.csv.xlsx`) is not included in this repo. Add it to the project root before running the notebook. If using the `.xlsx` version, load it with `pd.read_excel("spotify.csv.xlsx")` instead of `pd.read_csv()`.

## 🧹 Data Cleaning

The notebook performs several cleaning steps:

- Inspects missing values across the dataset.
- Normalizes inconsistent casing in the `explicit` column (`FAlSE`, `falSE`, `False`, etc.) into a proper boolean.
- Removes rows containing missing values.
- Removes the unnecessary `Unnamed: 0` index column.
- Converts `popularity` into a numeric format (`pd.to_numeric`, with mean-fill for any coerced NaNs).
- Converts `explicit` into an integer representation (0/1).
- Examines high-cardinality categorical columns such as artists, albums, and track names.
- Drops the high-cardinality/identifier columns (`track_id`, `artists`, `album_name`, `track_name`) from the modeling dataset.
- Encodes `track_genre` using `LabelEncoder` into `track_genre_encoded`.

### Cleaned Dataset

The dataset is reduced from:
`119,000 rows × 21 columns`

to:
`68,430 rows × 20 columns` after removing rows with missing values and the `Unnamed: 0` index column,

and further reduced to `68,430 rows × 16 numeric columns` after dropping identifier/high-cardinality text columns, ready for modeling.

## 🔎 Exploratory Data Analysis

The project explores:

- Missing-value patterns
- Distribution of Spotify audio features (histograms + KDE)
- Track popularity
- Genre distribution (count plot of encoded genres)
- Correlation heatmap of numerical features
- Categorical feature cardinality

## 🧠 Machine Learning: Genre Classification

- **Target**: `track_genre` (label-encoded as `track_genre_encoded`)
- **Features**: Numerical Spotify audio features (danceability, energy, loudness, etc.)
- **Algorithm**: `RandomForestClassifier` (`n_estimators=100`, `class_weight='balanced'`)
- **Split**: 80% train / 20% test, stratified by genre
- **Evaluation**: Accuracy score, classification report (precision/recall/F1 per genre), and feature importance ranking

## 🎧 Playlist Recommendation System

A content-based recommender (`get_playlist_recommendations`) that:

1. Scales all numerical audio features with `StandardScaler`.
2. Computes **cosine similarity** between a seed track and every other track.
3. Returns the top-N most similar tracks, using the original dataset to display metadata (track name, artist, album, genre).

Example usage in the notebook: searching for a track (e.g. *"Bohemian Rhapsody"*) and generating a 10-track playlist of similar songs.

## 🛠️ Tech Stack

- Python
- Jupyter Notebook
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn

## 🚀 How to Run

Clone the repository:

```bash
git clone https://github.com/AbhayRaj2005/spotify-data-analysis.git
cd spotify-data-analysis
```

Install dependencies:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn
```

Add `spotify.csv` (or `spotify.csv.xlsx`) to the project root, then run the notebook:

```bash
jupyter notebook spotify.ipynb
```

## 📁 Project Structure

```
.
├── spotify.ipynb        # Main notebook: cleaning, EDA, modeling, recommendations
├── spotify.csv          # Dataset in CSV format (not included, add your own)
├── spotify.csv.xlsx     # Dataset in Excel format (optional, alternative to CSV)
└── README.md
```

## 📈 Future Improvements

- Try additional models (Logistic Regression, XGBoost) and compare performance.
- Hyperparameter tuning via `GridSearchCV`.
- Incorporate `artists`/`album_name` via embeddings instead of dropping them.
- Deploy the recommender as a simple web app (e.g. Streamlit).
