# Netflix User Similarity via Locality-Sensitive Hashing (LSH)

This project implements a scalable and memory-efficient pipeline to identify pairs of Netflix users with similar movie preferences using **Locality-Sensitive Hashing (LSH)** and **MinHash signatures**. The objective is to find user pairs whose **Jaccard Similarity** is greater than 0.5, based solely on the sets of movies they have rated (actual rating values are ignored).

## 🎯 Goal

Due to the large size of the dataset — 100,000 users, 17,770 movies, and over 65 million user-movie rating records — brute-force computation of ~5 billion user pairs is infeasible. Therefore, this project uses LSH with MinHashing to efficiently reduce the number of comparisons while preserving the quality of similar pair detection.

This implementation adheres to the following constraints:
- Maximum memory usage: **8 GB RAM**
- Maximum runtime: **30 minutes**
- Solution must be reproducible using a **random seed passed as a command-line argument**
- We were able to keep the RAM below 5GB and time as average 6mins which is significantly less than the maximum runtime
  
This repository was built as part of the **Final Assignment for the Advances in Data Mining** course and completed within the assigned deadline of one week.

---

## 📁 Files

- `netflix_user_similarity.ipynb` – Complete Jupyter notebook with data loading, MinHash, LSH, candidate pair generation, and final similarity verification
- `final_report_lsh_netflix.pdf` – 5-page report describing methodology, design decisions, results, and analysis
- `README.md` – This documentation file

---

## 📦 Dataset

The dataset is a cleaned version of the **Netflix Prize Challenge** dataset, containing users who rated at least 300 and at most 3000 movies.

You can download the `.npy` data file (used for this implementation) from the following link:

📥 **[Download user_movie_rating.npy (Google Drive)](https://drive.google.com/file/d/1Fqcyu9g6DZyYK_1qmjEgD1LlGD7Wfs5G/view?usp=sharing)**

Each row contains:
[user_id, movie_id, rating]


> ⚠️ The actual rating value is ignored — only the fact that a user rated a movie is used.

---

## ⚙️ Implementation Highlights

- **CSR Matrix** (`scipy.sparse`) used to store user-movie interactions compactly
- **NumPy** arrays for storing movie indices per user
- **MinHash**: Signature length of 100 using random permutations
- **LSH Banding**: 20 bands, 5 rows per band (b = 20, r = 5)
- **MD5 Hashing** for stable bucketing
- **Thresholds**:
  - Estimated Jaccard similarity (via MinHash) ≥ 0.4 for candidate selection
  - Exact Jaccard similarity > 0.5 for final similar pair verification

---

## ⏱️ Runtime & Performance

| Metric                    | Value                        |
|--------------------------|------------------------------|
| Total runtime            | ~6 minutes                   |
| Memory usage             | ~6 GB (peak)                 |
| Signature length         | 100                          |
| Similar pairs found      | ~544                         |
| Candidate pairs evaluated| ~26 million                  |

Performance remained consistent across multiple random seeds.

---

## 🧠 How to Run

# If converting to Python script:
python netflix_user_similarity.py --seed 1234

# If using Jupyter:

Set the random seed at the top of the notebook (e.g. np.random.seed(1234)).
Run all cells sequentially.

# References

Leskovec, Rajaraman, Ullman — Mining of Massive Datasets
http://www.mmds.org
Netflix Prize Dataset — https://www.netflixprize.com

# Notes

This project was done to demonstrates how thoughtful data representation, approximation techniques, and efficient use of memory can make large-scale similarity detection feasible on limited resources. It was implemented in under a week as part of a final assignment and rigorously tested to meet runtime and memory constraints
