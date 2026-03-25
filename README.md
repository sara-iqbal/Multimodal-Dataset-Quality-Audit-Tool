# Multimodal Dataset Quality Audit Tool

This project provides a comprehensive pipeline for auditing the quality and diversity of multimodal datasets, specifically those containing image and text pairs. It is designed to help researchers and engineers ensure their data is AI-ready by identifying low-quality samples, measuring caption-image alignment, and removing duplicates.

## Project Links

* **Interactive Dashboard**: [sara-iqbal.github.io/Multimodal-Dataset-Quality-Audit-Tool/](https://sara-iqbal.github.io/Multimodal-Dataset-Quality-Audit-Tool/)
* **Google Colab Notebook**: [View Source Code](https://colab.research.google.com/drive/1rSHleRaiEe0upfxoZaje-6E9VAbOr8iv#scrollTo=npvDRtDZCy7k)

---

## The Problem

Large-scale multimodal datasets often suffer from significant noise that can degrade the performance of models like CLIP or Stable Diffusion. Common issues include:

* **Textual Noise**: Captions that are too short, written in all-caps, contain URLs, or lack linguistic variety.
* **Image Quality Issues**: Low-resolution images, extreme aspect ratios, or incorrect color modes (e.g., grayscale instead of RGB).
* **Poor Alignment**: Captions that do not actually describe the visual content of the image.
* **Data Redundancy**: Duplicate or near-duplicate pairs that lead to overfitting and wasted computational resources.

---

## The Solution

This tool automates the audit process by ingestion a dataset and running it through a multi-stage quality scoring engine. By quantifying "quality" into a 0-100 score, it allows users to filter their data into High, Medium, and Low-quality tiers. 

The implementation uses the MS-COCO dataset as a benchmark, processing thousands of samples to demonstrate how raw data can be transformed into a curated, high-quality subset for machine learning.

---

## Methodology

The audit pipeline is divided into several distinct stages:

### 1. Data Ingestion
The system processes image metadata (width, height, format, mode) alongside their corresponding text captions. In the Colab implementation, data is fetched directly from official COCO repositories and parsed locally to bypass security restrictions related to remote loading scripts.

### 2. Text Quality Audit
Captions are evaluated based on three primary metrics:
* **Shannon Entropy**: Measures the character-level randomness to detect "gibberish" or repetitive patterns.
* **Repetition Ratio**: Analyzes word-level uniqueness to flag redundant phrasing.
* **Technical Flags**: Automatically flags samples containing URLs, non-ASCII characters, or inappropriate formatting like all-caps.

### 3. Image Quality Audit
Images are scored based on their utility for modern computer vision models:
* **Resolution**: Prefers images above 224x224 pixels.
* **Aspect Ratio**: Scores standard landscape, portrait, and square ratios higher than extreme, "sliver" images.
* **Color Mode**: Prioritizes RGB images over grayscale or unusual formats.

### 4. Caption-Image Alignment
A descriptive proxy scoring system checks for the presence of visual keywords (e.g., "person," "vehicle," "outdoor") within the captions. This ensures that the text provides high-level descriptive value relative to the image.

### 5. Deduplication and Tiering
The final stage performs exact-match and near-duplicate removal using MD5 hashing and prefix matching. Remaining samples are then assigned to one of four tiers:
* **High**: Scores above 70; ideal for final training.
* **Medium**: Scores 45-70; useful for pre-training with augmentation.
* **Low**: Scores below 45; recommended for removal.
* **Flagged**: Samples with severe technical errors.

---

## Technical Stack

* **Python**: Core programming language.
* **Pandas & NumPy**: Data manipulation and numerical analysis.
* **Matplotlib**: Generation of the 9-panel audit dashboard and distribution plots.
* **Pillow (PIL)**: Image metadata extraction.
* **Hugging Face Datasets**: Initial data sourcing.

---

## How to Run

1.  Open the Google Colab link provided above.
2.  Run the initialization cells to download the COCO validation set and annotations.
3.  Execute the audit cells sequentially to generate the quality scores.
4.  The final cell will export a `multimodal_audit_data.json` file and a `multimodal_audit.png` dashboard.
