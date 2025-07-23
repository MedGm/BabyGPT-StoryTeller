# 🦊 BabyGPT-StoryTeller

──────────────────────────────────────────────────────────────────────────────

🤗 A fine-tuned GPT-2 model that generates safe, coherent, and creative 
   children’s stories, trained on the TinyStories dataset.

Perfect for educational apps, bedtime story generators, and storytelling tools 
for children.

──────────────────────────────────────────────────────────────────────────────

## 📖 About

BabyGPT is a compact and creative text generation model based on GPT-2, fine-tuned to output imaginative short stories designed for young readers. Inspired by the simplicity of the TinyStories dataset, this project showcases how targeted fine-tuning can yield expressive, safe, and engaging narratives — even on limited hardware.

### ✅ Key Features
-  **Child-Safe Language** — Generates age-appropriate content for children 4–8 years old.
-  **Story-Coherent** — Learns consistent beginnings, middles, and ends.
-  **Lightweight** — Fine-tuned on Google Colab using GPT-2 117M with 5% TinyStories.
-  **Hallucination-Resistant** — Controlled sampling for safe, meaningful outputs.

---

## 🛠️ Technologies Used

-  Transformers (Hugging Face)
-  PyTorch
-  GPT-2 (117M parameters)
-  TinyStories Dataset
-  Google Colab (T4 GPU)

---

## 🚀 Quick Start

```python
from transformers import pipeline, AutoTokenizer, AutoModelForCausalLM
from huggingface_hub import login

# Auth for private repo (optional if public)
login("your-huggingface-token")

# Load model + tokenizer
model_id = "MedGm/babygpt-storyteller"
tokenizer = AutoTokenizer.from_pretrained(model_id)
model = AutoModelForCausalLM.from_pretrained(model_id)

storyteller = pipeline("text-generation", 
                       model=model, 
                       tokenizer=tokenizer,
                       device=-1)  # Force CPU

# Prompt format
prompt = "Title: The Dancing Robot\nStory:\nOnce upon a time, there was a robot who"

# Generate story
outputs = storyteller(
    prompt,
    max_new_tokens=200,
    temperature=0.8,
    top_k=50,
    repetition_penalty=1.2,
    do_sample=True,
    truncation=True,
    eos_token_id=tokenizer.eos_token_id  # Add if your tokenizer supports it
)

story = outputs[0]["generated_text"]
print("\n Story Generated:\n")
print(story)

```

| Metric            | Value                                  |
| ----------------- | -------------------------------------- |
| **Training Loss** | 1.6925                                 |
| **Training Time** | 1h 31m                                 |
| **Samples Used**  | \~106,000                              |
| **Epochs**        | 2                                      |
| **BERTScore**     | 0.867 (on test stories)                |
| **Model Size**    | 124M (GPT-2 117M + tokenizer overhead) |


## 📚 Dataset: TinyStories
- Curated dataset of high-quality, short children’s stories
- Clean grammar, small vocabulary, structured narrative
- Ideal for lightweight fine-tuning and language modeling

## Example Outputs
Prompt:
"Once upon a time, there was a tiny robot with a big dream."

Generated:

"...He wanted to fly to the moon and paint the stars with colors. Every day, he worked hard in his garden, building wings out of metal and hope. One day, his dream came true..."

Prompt:
"The little rabbit discovered a glowing mushroom..."

Generated:

"...It whispered secrets about the forest. The rabbit listened carefully, then hopped away to share the magic with her friends. From that day, the forest sparkled with kindness."

