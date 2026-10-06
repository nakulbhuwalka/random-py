# test-dec-model

Evaluates a custom Ollama model (`tev1`) on IMDB movie-review sentiment classification.

Each review is sent to the model via `ollama.systemone` with one question ("Is the this a positive movie review?"). The model returns a score (`noul`) between 0 and 1; scores above 0.5 are classified as positive.

Dataset: [IMDB Dataset of 50K Movie Reviews](https://www.kaggle.com/datasets/lakshmi25npathi/imdb-dataset-of-50k-movie-reviews) (Kaggle).

## Requirements

- Python 3.12+
- [uv](https://docs.astral.sh/uv/)
- An Ollama server with the `tev1` model, and an `ollama` client that supports `systemone`
- A 5,000-review sample of the dataset at `~/data/imdb_5000.csv`, with a header row, review text in column 1 and the label (`positive`/`negative`) in column 2

## Setup

```sh
uv sync
uv run jupyter notebook
```

## Usage

Open [test-imdb.ipynb](test-imdb.ipynb) and run all cells. It:

1. Defines `review_is_positive(review)`, which calls `tev1` and returns the `is_pos` answer.
2. Classifies every row of `~/data/imdb_5000.csv`, recording `-1` / `failed` if a request errors.
3. Prints the total execution time and writes the results to [result.csv](result.csv).

## Results

[result.csv](result.csv) has one row per review:

| Column | Description |
| --- | --- |
| `is_positive` | Model score in [0, 1], or `-1` if the request failed. |
| `sentiment` | Ground-truth label from the dataset. |
| `calc_sentiment` | `positive` or `negative` from the 0.5 threshold, or `failed`. |

In the saved run, 4,786 of 4,999 reviews (about 95.7%) matched the label, and 3 requests failed.
