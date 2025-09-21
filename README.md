# Image-Captioning

# 🖼️ Image Captioning with EfficientNet-B3 + Attention LSTM  

This repository contains an implementation of an **image captioning system** that generates natural language descriptions for images.  
It uses a **CNN encoder (EfficientNet-B3)** for extracting visual features and a **decoder (LSTM with attention)** to produce captions.  
Performance is evaluated using **BLEU** and **METEOR** scores on the COCO 2014 dataset subset.  

---

## 📌 Features
- Preprocessing pipeline for captions (cleaning, tokenization, vocabulary building).  
- Feature extraction with **EfficientNet-B3** (pretrained on ImageNet).  
- Attention-based **LSTM decoder** for generating captions.  
- Training with **label smoothing** and masking for padded tokens.  
- Inference with **beam search decoding** (width=5).  
- Evaluation using **BLEU (1–4)** and **METEOR** metrics.  

---


---

## ⚙️ How It Works

### 1. Data Preprocessing
- Captions are **lowercased**, punctuation removed, and normalized.  
- Special tokens `<start>` and `<end>` are added to mark sentence boundaries.  
- Vocabulary size: **~4,500 words**.  
- Captions padded to a maximum length of **20 tokens**.  

---

### 2. Encoder: EfficientNet-B3
- **EfficientNet-B3** is a convolutional neural network known for balancing **accuracy and efficiency** by scaling depth, width, and resolution together.  
- Pretrained weights on **ImageNet** are used (encoder frozen).  
- Extracted features: **10×10×1536**, reshaped to **100×256** vectors for the attention module.  

👉 This provides rich visual features without the need for training from scratch.  

---

### 3. Decoder: LSTM + Attention
- Word embeddings of size **200**.  
- **LSTM** with hidden size **512**, initialized from global image features.  
- **Attention mechanism**: focuses on relevant image regions for each generated word.  
- Final dense layer predicts next word over the vocabulary.  

👉 Attention improves the model’s ability to describe specific objects and scenes.  

---

### 4. Training Details
- **Loss**: Cross-Entropy with **Label Smoothing (ε=0.1)**.  
- **Masking**: padding tokens are ignored during loss calculation.  
- **Optimizer**: Adam with learning rate `1e-4`.  
- **Early stopping** at epoch ~43 (best validation loss ≈ 3.49).  

---

### 5. Inference
- Captions generated with **Beam Search (beam width=5)**.  
- Compared to greedy search, beam search explores multiple hypotheses and selects the most fluent caption.  

---

## 📊 Evaluation Metrics

### BLEU (Bilingual Evaluation Understudy)
- Measures **n-gram overlap** between predicted captions and references.  
- BLEU-1 (unigram) → BLEU-4 (4-grams).  
- Scores from this model:  
  - BLEU-1: **0.71**  
  - BLEU-2: **0.53**  
  - BLEU-3: **0.37**  
  - BLEU-4: **0.26**  

### METEOR (Metric for Evaluation of Translation with Explicit ORdering)
- Considers **synonyms, stemming, and recall**, making it more semantic than BLEU.  
- Score from this model: **0.40**.  

---

## 🔮 Example Predictions
- **Image**: clock tower  
  - **Reference**: “A clock on top of a church steeple with a blue sky in the background.”  
  - **Prediction**: “a clock tower with a clock on it”  

- **Image**: person with a Wii controller  
  - **Reference**: “A woman smiling standing near a man holding a remote.”  
  - **Prediction**: “a woman holding a wii controller in his hand”  

---

## 🚀 Future Work
- Fine-tune last layers of EfficientNet-B3 instead of freezing.  
- Try transformer-based decoders.  
- Add more metrics: **CIDEr** and **SPICE**.  
- Visualize attention heatmaps to interpret which image regions the model attends to.  

---

## 📦 Installation & Usage

1. Clone the repo:
```bash
git clone https://github.com/your-username/image-captioning.git
cd image-captioning

