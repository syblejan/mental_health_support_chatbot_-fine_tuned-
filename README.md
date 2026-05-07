# 🧠 Mental Health Support Chatbot (Fine-Tuned GPT-Neo)

> An empathetic AI chatbot fine-tuned on emotional dialogues to provide compassionate mental health support conversations.

---

## 📌 Task Objective

The goal of this task was to fine-tune a pre-trained large language model to act as an empathetic mental health support chatbot. The chatbot is designed to understand a user's emotional state and respond with warmth and understanding, simulating a supportive conversational partner.

---

## 📂 Dataset Used

**Dataset:** [`empathetic_dialogues`](https://huggingface.co/datasets/empathetic_dialogues)  
**Source:** Hugging Face Datasets Hub  
**Description:** A dataset of 25,000+ conversations grounded in emotional situations, where each dialogue is labeled with one of 32 emotion categories (e.g., *joyful*, *anxious*, *devastated*, *caring*).

| Split      | Subset Used |
|------------|-------------|
| Train      | 5,000 examples (shuffled) |
| Validation | 500 examples (shuffled) |

Each example contains:
- `context` — the emotion label
- `prompt` — the user's situation/message
- `utterance` — the empathetic response

---

## 🤖 Model Applied

**Base Model:** [`EleutherAI/gpt-neo-1.3B`](https://huggingface.co/EleutherAI/gpt-neo-1.3B)  
**Fine-Tuning Method:** **LoRA (Low-Rank Adaptation)** via the `peft` library

### Why LoRA?
GPT-Neo 1.3B has over 1.3 billion parameters — far too large to fully fine-tune in a constrained environment like Google Colab. LoRA injects small trainable adapter layers into the model, reducing the number of trainable parameters by ~99% while preserving model quality.

### LoRA Configuration

| Parameter           | Value         |
|---------------------|---------------|
| Rank (`r`)          | 8             |
| LoRA Alpha          | 32            |
| LoRA Dropout        | 0.1           |
| Task Type           | Causal LM     |

### Training Configuration

| Parameter                     | Value              |
|-------------------------------|--------------------|
| Learning Rate                 | 2e-4               |
| Train Batch Size              | 1                  |
| Gradient Accumulation Steps   | 8                  |
| Epochs                        | 3                  |
| Max Token Length              | 128                |
| Mixed Precision               | FP16               |

---

## 💬 Conversation Format

The model was trained using a structured prompt format:

```
Emotion: caring
User: I've been feeling really overwhelmed lately.
Bot: I'm so sorry to hear that. It's completely okay to feel that way...
```

At inference time, the emotion is fixed to `caring` to elicit warm, supportive responses.

---

## 📊 Key Results & Findings

- **Parameter Efficiency:** LoRA reduced trainable parameters from ~1.3B to roughly **3–5M**, making fine-tuning feasible on a free-tier GPU (Colab T4).
- **Empathetic Tone:** The model successfully learned to respond in an emotionally aware manner, adapting its language to the emotional context provided.
- **Repetition Mitigation:** Post-generation fixes including `repetition_penalty=1.2` and `no_repeat_ngram_size=2` significantly reduced looping and prompt echoing.
- **Dataset Artifact Handling:** The `empathetic_dialogues` dataset encodes commas as `_comma_` — a cleanup step was applied post-generation to produce natural-sounding output.

### Challenges & Observations
- Small batch size (1) with gradient accumulation was necessary to fit within GPU memory limits.
- The model occasionally produces short or generic responses; increasing `max_new_tokens` or training for more epochs could improve response depth.
- Temperature tuning (`0.6`) and nucleus sampling (`top_p=0.9`) provided a good balance between creativity and coherence.

---

## 🛠️ Tech Stack

| Tool / Library     | Purpose                          |
|--------------------|----------------------------------|
| `transformers`     | Model loading, training, tokenization |
| `peft`             | LoRA fine-tuning                 |
| `datasets`         | Dataset loading & preprocessing  |
| `torch`            | Deep learning backend            |
| Google Colab       | Training environment (T4 GPU)    |

---


---


```

---



- [EleutherAI](https://www.eleuther.ai/) for the GPT-Neo model
- [Facebook AI Research](https://ai.meta.com/) for the Empathetic Dialogues dataset
- [Hugging Face](https://huggingface.co/) for the `transformers`, `peft`, and `datasets` libraries

---

*Built as part of an AI/ML Internship Program.*
