# 📝 Recipe Generation – Late Plate AI Models

This branch contains the AI models responsible for generating personalized cooking recipes based on user-provided ingredients and context. It is part of the **Late Plate** smart cooking assistant system.

---

## 📦 Dataset

**RecipeNLG Dataset**  
A large-scale, structured dataset containing over 2 million cooking recipes, each with:
- Title
- Ingredients
- Instructions
- Metadata (e.g., cuisine, course type)

Used for training and evaluating the generative models for both:
- **Recipe generation**
- **Recipe recommendation**

Dataset Source: [RecipeNLG documentation](https://www.researchgate.net/publication/345308878_Cooking_recipes_generator_utilizing_a_deep_learning-based_language_model)

---

## 🤖 Models

### 🔹 GPT-2 Small
A baseline autoregressive transformer model with:
- Positional embeddings
- Multi-head self-attention
- Feed-forward layers
- Layer normalization

#### ⚙️ Fine-tuning
- Batch size: `10,000`
- Custom tokenizer with special tokens
- Training done in iterative batches with checkpointing
- Trained for **4 epochs**

| Learning Rate | Training Loss |
|---------------|---------------|
| `5e-4`        | 3.70          |
| `5e-5`        | **0.58**      |

---

### 🔹 LLaMA 3.2 1B
Modern transformer-based architecture with:
- Rotary positional encodings
- Grouped-query attention
- RMSNorm normalization
- Gated feed-forward layers

#### ⚙️ Fine-tuning
- Batch size: `4,000`
- Used [LoRA (Low-Rank Adaptation)](https://arxiv.org/abs/2106.09685) via the **Unsloth** library
- Training split across 4 notebooks due to memory constraints
- Final model merged post-training

| Learning Rate | Training Loss |
|---------------|---------------|
| `2e-4`        | 2.33          |
| `2e-5`        | **0.77**      |

---

## 🧪 Evaluation

Model performance was evaluated using **BERTScore-F1**, a semantic similarity metric.

| Model          | Learning Rate | Epochs | Time per Epoch (hrs) | BERTScore-F1 |
|----------------|---------------|--------|-----------------------|---------------|
| LLaMA 3.2 1B   | `2e-4`        | 1      | 415                   | **0.9351**    |
| GPT-2 Small    | `5e-5`        | 1      | 64                    | 0.7913        |
| GPT-2 Small    | `5e-5`        | 4      | 64                    | 0.8981        |

- **LLaMA 3.2 1B** achieved the best BERTScore-F1 after a single epoch.
- **GPT-2 Small** showed consistent improvement over multiple epochs.

---

## 🛠️ Dependencies

Key libraries used:
- `transformers`
- `datasets`
- `unsloth`
- `accelerate`
- `torch`
- `scikit-learn`
- `bert-score`

---

## 📜 License

This project is licensed for academic and research use only.  
Please refer to the individual model or dataset licenses before redistributing or using in production.

## 📬 Contact

This model is part of the [Late Plate](https://github.com/Late-Plate)  
For questions, issues, or contributions, please open an issue or visit the main organization page.



