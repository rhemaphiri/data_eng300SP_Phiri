# Homework 2 — MovieLens 1M Recommendation System

## Overview
This notebook builds a movie recommendation system using the MovieLens 1M dataset and BERT embeddings. It covers downloading the dataset, generating embeddings, and recommending movies for different user types.

---

## Requirements

### AWS Credentials
Before running any task, set your AWS credentials in the import cell, specifically the access key id, secret access key and session token as without it, you cannot access the bucket

---

## Configuration
At the top of the notebook, set your S3 bucket name and data directory:
```python
BUCKET_NAME = "your-bucket-name"
DATA_DIRECTORY = "dataset/moviedata/ml-1m"
```

---

## How to Run

Run the tasks in order by executing each cell top to bottom. Each task has a pipeline function that can be called directly.

---

## Task Descriptions & Expected Outputs

### Task 1 — Download MovieLens 1M Dataset
**Function:** `task1_pipeline()`

Downloads the MovieLens 1M dataset and uploads it to S3. Skips the download if the files already exist in the S3 bucket.

**Expected Output:**
```
All files already in S3.
# or if not in S3:
Dataset saved to dataset/moviedata
  Uploaded: ML-1M_dataset/ratings.dat
  Uploaded: ML-1M_dataset/movies.dat
  Uploaded: ML-1M_dataset/users.dat
  Uploaded: ML-1M_dataset/README
All files uploaded successfully.
```

**S3 Output:** `s3://<bucket>/ML-1M_dataset/` containing `ratings.dat`, `movies.dat`, `users.dat`, `README`

---

### Task 2 — BERT Embeddings for Pre-1980 Movies (Offline Step)
**Function:** `task2_pipeline()`

Filters movies released in 1980 or earlier, generates BERT embeddings using `distilbert-base-uncased`, and uploads the embeddings to S3. This is an offline step — embeddings only need to be created once.

**Expected Output:**
```
Number of movies: 887
Embedding shape: (887, 768)
Embeddings already in S3. Skipping upload.
# or if not in S3: uploads movie_embeddings.csv and movie_ids.npy
```

**S3 Output:** `s3://<bucket>/embeddings/movie_embeddings.csv`, `s3://<bucket>/embeddings/movie_ids.npy`

---

### Task 3 — Recommendations for Pre-1980 Movies
**Function:** `task_pipeline(pre1980_only=True)`

Recommends 5 movies for two user types using BERT + FAISS similarity search:
- **Cold user** — no history, receives the 5 most popular pre-1980 movies
- **Top user** — a randomly selected user from the top 5% by number of ratings, receives 5 movies similar to their most recently rated movies

**Expected Output:**
```
  Cold recs: ['Star Wars: Episode IV - A New Hope (1977)', 'Godfather, The (1972)', ...]
  Top recs: ['Champ, The (1979)', 'Gods Must Be Crazy, The (1980)', ...]
  Uploaded to s3
```

**S3 Output:** `s3://<bucket>/recommendations/recommendations_pre1980.csv`

CSV columns: `User_Type`, `Last_Interaction_Time`, `N_Interactions`, `Avg_Rating`, `UserID`, `Gender`, `Age`, `Occupation`, `Rec_1_ID`, `Rec_1_Title`, `Rec_1_Year`, `Rec_1_Genres`, ... `Rec_5_*`

---

### Task 4 — Recommendations for Full Dataset
**Function:** `task_pipeline(pre1980_only=False)`

Repeats Task 3 using the full MovieLens dataset instead of only pre-1980 movies. Reuses all the same functions with batched embedding generation as the kernel crashes when you attempt to generate all embeddings at once.

**Expected Output:**
```
  Cold recs: [top 5 most popular movies across full dataset]
  Top recs: [5 movies similar to top user's history]
  Uploaded to s3
```

**S3 Output:** `s3://<bucket>/recommendations/recommendations_full.csv`

---

### Task 5 — Personal User Profile & Recommendations
**Function:** `task5_pipeline()`

Creates a personal user profile from 10 manually rated movies, builds a weighted BERT embedding profile, and recommends 5 movies using FAISS similarity search. Ratings are weighted — higher rated movies influence the profile more. Personal ratings are included in the ipynb file.

**Expected Output:**
```
Saved to s3
   ID                  Title  Year                  Genres
  Boomerang (1992)  ...
```

**S3 Output:**
- `s3://<bucket>/my_profile/my_profile.csv` — personal ratings
- `s3://<bucket>/my_profile/my_recommendations.csv` — 5 recommended movies

---

## Helper Function Reference

| Function | Task | Description |
|---|---|---|
| `s3_prefix_exists(bucket, prefix)` | 1 | Checks if all ML-1M files exist in S3 |
| `download_movielens_1m(data_dir)` | 1 | Downloads and extracts ML-1M zip |
| `upload_to_s3(local_dir, bucket, prefix)` | 1 | Uploads dataset files to S3 |
| `task1_pipeline()` | 1 | Runs full download + upload pipeline |
| `load_movies_before_1980(data_dir)` | 2 | Loads movies filtered to <= 1980 |
| `bert_embed(texts)` | 2 | Generates BERT embeddings for a list of texts |
| `bert_embed_batched(texts)` | 4 | Batched version of bert_embed for large datasets |
| `generate_bert_embeddings(movies)` | 2 | Embeds all movies and builds FAISS index |
| `save_and_upload_embeddings(...)` | 2 | Saves embeddings CSV and uploads to S3 |
| `task2_pipeline()` | 2 | Runs full embedding generation pipeline |
| `load_ratings(data_dir)` | 3 | Loads ratings.dat |
| `load_users(data_dir)` | 3 | Loads users.dat |
| `load_all_movies(data_dir)` | 4 | Loads all movies without year filter |
| `load_embeddings()` | 3 | Loads saved embeddings from disk |
| `build_user_text(user_id, ...)` | 3 | Gets last N rated movies as text for a user |
| `build_faiss_index(movie_vecs)` | 3 | Builds FAISS inner product index |
| `recommend(user_id, ...)` | 3 | Recommends k movies for a user |
| `get_top_user(ratings, users)` | 3 | Selects a random user from top 5% by rating count |
| `save_recs(records, ...)` | 3 | Saves recommendations CSV and uploads to S3 |
| `task_pipeline(pre1980_only)` | 3/4 | Runs full recommendation pipeline |
| `build_my_profile(my_ratings, ...)` | 5 | Builds weighted BERT profile from personal ratings |
| `recommend_for_me(my_ratings, ...)` | 5 | Recommends movies based on personal profile |
| `save_profile_and_recs(...)` | 5 | Saves profile and recommendations to S3 |
| `task5_pipeline()` | 5 | Runs full personal recommendation pipeline |
