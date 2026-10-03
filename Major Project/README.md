# Reel Match: Movie Recommendation System

Reel Match is a content-based movie recommender built with TF-IDF and cosine
similarity. The current runnable workflow is in `ReelMatch.ipynb`: it loads and
filters the dataset, trains the model, saves artifacts, starts a Flask server,
and provides a small web UI.

## How it works

- The notebook reads `TMDB  IMDB Movies Dataset.csv` from the project folder.
- It filters for released, non-adult movies with a poster and usable overview,
  removes duplicates and low-vote-count entries, then keeps the top 12,000 by
  a popularity/vote-count ranking.
- TF-IDF features use each movie's overview, tagline, genres, keywords,
  director, and up to five cast members. Nearest Neighbors ranks results by
  cosine distance.
- The notebook saves the trained model as compressed files in
  `backend/model_artifacts/` and uses the in-memory model to serve requests.
- Movie details come from the dataset. Poster images use TMDB's public image
  CDN; no API key is required.

## Project structure

```
Major Project/
├── ReelMatch.ipynb                 # full training and local app workflow
├── TMDB  IMDB Movies Dataset.csv   # local input; excluded from Git
├── backend/
│   ├── Procfile                    # currently expects the missing app.py
│   ├── requirements.txt
│   ├── runtime.txt
│   ├── model_artifacts/            # saved model files
│   └── static/index.html           # web UI
├── docs/                           # API, dataset, model notes, screenshots
├── train/prepare_and_train.ipynb   # training notebook
└── README.md
```

The dataset is excluded by `.gitignore`, so it must be available locally before
running the full notebook. The notebook expects the exact filename shown above
in the project folder.

## Run locally

Install the backend dependencies and Jupyter in the Python environment you
will use for the notebook:

```bash
cd "Major Project"
python -m pip install -r backend/requirements.txt jupyterlab
jupyter lab ReelMatch.ipynb
```

Run the notebook from top to bottom. Its dependency-install cell runs first;
if an import such as `flask_cors` is still missing, install into the active
notebook kernel with `%pip install -r backend/requirements.txt` and rerun the
cell. The Flask server starts in a background thread at `http://127.0.0.1:5000`.
Keep the notebook kernel running while using the app.

## API

The API is available after the Flask cell has run:

| Endpoint           | Method | Purpose                                                    |
| ------------------ | ------ | ---------------------------------------------------------- |
| `/health`          | GET    | Returns server status and number of movies loaded.         |
| `/movies?q=<text>` | GET    | Searches movie titles for autocomplete.                    |
| `/recommend`       | POST   | Accepts `{"movie": "<title>"}` and returns similar movies. |

Example:

```bash
curl -X POST http://127.0.0.1:5000/recommend \
  -H "Content-Type: application/json" \
  -d '{"movie": "Inception"}'
```

See `docs/API_TESTING.md` and `docs/Reel_Match.postman_collection.json` for
additional API checks.

## Deployment status

The current project does not have `backend/app.py`. The `backend/Procfile`
still starts `gunicorn app:app`, so the current notebook-based project cannot
be deployed using that Procfile. The notebook also binds Flask to
`127.0.0.1`, which is intended for local use. To deploy, first restore or
create a standalone backend entry point that loads the saved artifacts and
binds to the host and port required by the hosting service; then update and
test the Procfile and deployment instructions.
