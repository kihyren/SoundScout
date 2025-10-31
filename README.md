# SoundScout
A small music search + recommendation app that tries to act like a mini Spotify, but on a tiny dataset.

## 1. What it is
SoundScout lets a user:
1. type a song/artist they’re looking for,
2. see songs that are **similar** (not just exact matches),
3. pick the one that “feels closest,” and
4. use that feedback to make future suggestions better.

Goal: show how search, recommendations, and a bit of ML can work together on a small, custom dataset.

## 2. Why I built it
Most music recommenders are huge and kind of a black box. I wanted a version that:
- Is personalised to the user
- Uses ML to create recommendations based on past searches
- I can swap models in/out easily so it can grow and update

This project shows: **search → ranking → user feedback → improved ranking**.

## 3. High-Level Design
**Step 1: Collect data**  
I pull a small set of songs from public/sample sources on the internet (title, artist, genre, maybe URL). Then I clean it and store it in a simple database (SQLite to start).

**Step 2: Search**  
When the user types something like `“chill r&b like daniel caesar”`, I turn that text into numbers (vectorize it) and compare it to all songs in the database. The closest ones are returned first.

**Step 3: Feedback**  
The user can click “this is the closest.” I save that as feedback so I can prioritize songs with similar attributes later.

## 4. Tech Stack
- **Language:** Python
- **Backend / API:** FastAPI (or Flask — easy to swap)
- **ML / Search:** scikit-learn (TF-IDF, k-NN), pandas
- **Database:** SQLite (simple file-based DB for local runs)
- **Data/ETL scripts:** Python scripts to fetch → clean → load songs

## 5. Core Features
- **Search songs** by title/artist/keywords.
- **See similar songs** using vector similarity (k-nearest neighbors).
- **Give feedback** on the best match.
- **Store search + feedback** so recommendations can get better.
- **Data pipeline** to add more songs later.

## 6. Roadmap
**Phase 0 – Setup**  
- [ ] Create FastAPI app  
- [ ] Add SQLite DB + sample `songs.csv`  
- [ ] Endpoint: `GET /health`

**Phase 1 – Basic Search (MVP)**  
- [ ] Load songs into DB  
- [ ] Build TF-IDF model over song fields (title, artist, tags)  
- [ ] Endpoint: `GET /search?q=...` → return top 10 songs

**Phase 2 – Similar Songs**  
- [ ] Endpoint: `GET /songs/{id}/similar`  
- [ ] Use k-NN over stored vectors

**Phase 3 – Feedback Loop**  
- [ ] Endpoint to submit “this was the closest”  
- [ ] Save to `feedback` table  
- [ ] Re-rank search results to boost songs users said were good matches

**Phase 4 – Data Engineering Pass**  
- [ ] Script to pull songs from multiple sources  
- [ ] Clean/normalize (artist names, genres)  
- [ ] Optional: add embeddings for better similarity

**Phase 5 – Polish**  
- [ ] Simple UI (search bar + results)  
- [ ] Dockerfile  
- [ ] Basic tests

<!-- How to talk about it in an interview
- **Problem:** “I wanted a music recommender I can actually explain — not just ‘I called Spotify’s API.’”
- **Approach:** “I built a search-first system, then layered recommendations, then added user feedback.”
- **ML part:** “I started simple with TF-IDF + k-NN, so I can later swap in better embeddings.”
- **Data engineering part:** “I treat song data like a mini pipeline: collect → clean → store → index.”
- **Scalability story:** “Right now it’s SQLite + scikit-learn. If the dataset grows, I can move to Postgres and a vector DB.” -->

## 7. Possible Improvements
- Swap TF-IDF for sentence-transformer embeddings
- Add user profiles (store taste over time)
- Add playlist-style recommendations (“more like this set of songs”)
- Deploy to a small VM / container

## 8. License
MIT
