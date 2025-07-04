# ⭐ Recipe Recommendation – Late Plate AI Models

This branch contains the hybrid recommendation engine developed for the **Late Plate** mobile app. It suggests personalized recipes based on user preferences, past interactions, and recipe content using collaborative filtering, content-based filtering, and popularity models.

---

## 📦 Datasets

### 🔹 FoodRecSys V1
User-item interaction dataset with explicit ratings for various recipes.  
Used to train collaborative and hybrid models.

### 🔹 RecipeNLG Dataset
Used to extract content-based features like ingredients and cooking methods.

---

## 🤖 Models

### 🔹 Content-Based Filtering

Recommends recipes by analyzing their textual features and aligning them with user interests.

**Key Steps:**
1. **Recipe Profiles:** Built from ingredients and cooking methods.
2. **TF-IDF Vectorization:** Applied to emphasize unique recipe features.
3. **User Profiles:** Averaged TF-IDF vectors of highly rated (≥4) recipes.
4. **Neural Re-ranker:**
   - Dual-tower feedforward neural network:
     ```
     Dense(256, ReLU) → BatchNorm → Dropout(0.3) → Dense(128, ReLU) → Dense(64)
     ```
   - Dot product + sigmoid for prediction.

**Training Setup:**
- Binary classification (label = 1 if rating ≥ 4)
- Loss: Binary Cross Entropy
- Batch Size: 128
- Epochs: 20
- Early Stopping (patience=3)
- Evaluation: AUC

**Recommendation Logic:**
- Top 2000 candidates from cosine similarity
- Re-ranked using the neural network
- Final top-K returned

### 🔹 Collaborative Filtering – NeuMF (Neural Matrix Factorization)

Learns user-recipe interactions using both linear and non-linear pathways.

**Architecture:**
1. **Input:** User and recipe indices
2. **MF Pathway:** Dot product of low-dimensional embeddings
3. **MLP Pathway:** 
   - Embedding concat → Dense(64, ReLU) → BatchNorm → Dropout(0.3)
4. **Fusion Layer:** MF + MLP → Dense(32, ReLU) → Sigmoid

**Training:**
- Loss: Binary Cross Entropy
- Optimizer: Adam (LR = 0.001)
- Batch Size: 512
- Early stopping on AUC (patience = 5)
- LR reduction if AUC stagnates

**Recommendation:**
- Remove seen recipes
- Batch scoring of candidates
- Return top-K

### 🔹 Popularity-Based

Addresses cold-start users by recommending high-quality, widely rated recipes.

**Score Formula:**
weighted_score = (avg_rating × num_ratings) / (num_ratings + 1)


- Filters out recipes with <3 ratings
- Ranks recipes based on weighted score

### 🔹 Hybrid Filtering

Combines CF and CB predictions to improve coverage and precision.

**Logic:**
- For known users:
HybridScore = 0.95 × CF_Score + 0.05 × CB_Score
- Normalize scores and exclude seen items
- For new users: fallback to popularity model
- Recommend top-K items by final hybrid score

---

## 📊 Evaluation Results

### 🔸 Recall

| Model   | Top 5 | Top 10 | Top 20 |
|---------|--------|--------|--------|
| CB      | 9.4%   | 16.5%  | 31%    |
| CF      | 22%    | 39%    | 61%    |
| Hybrid  | 14.9%  | 24%    | 40.8%  |

### 🔸 Precision

| Model   | Top 5 | Top 10 | Top 20 |
|---------|--------|--------|--------|
| CB      | 5.8%   | 5.3%   | 5%     |
| CF      | 31%    | 28%    | 23%    |
| Hybrid  | 23%    | 19.3%  | 16.7%  |

- CF outperforms other models in both recall and precision.
- Hybrid model provides a strong trade-off between accuracy and generalization.

---

## 🛠️ Dependencies

Key libraries:
- `TensorFlow` / `Keras`
- `Scikit-learn`
- `Pandas`
- `NumPy`
- `Matplotlib`

---

## 📜 License

This project is licensed for academic and research use only.  
Please refer to the individual model or dataset licenses before redistributing or using in production.

## 📬 Contact

This model is part of the [Late Plate](https://github.com/Late-Plate)  
For questions, issues, or contributions, please open an issue or visit the main organization page.


