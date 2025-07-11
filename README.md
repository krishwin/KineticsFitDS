# 🏋️‍♂️ KineticsFitDS: An Attempt to create a Large Webscale DataSet for Fitness video clips similar to Kinetics400 but specific to fitness from YouTube Fitness Videos

An end-to-end pipeline for discovering, filtering, labeling, and analyzing YouTube videos related to **home fitness**, powered by NLP, Argilla, and BERT. This tool helps automatically classify relevant videos and extract trending topics from them.

---

## 🔄 Pipeline Overview

1. **YouTube Channel Discovery**
   - Use YouTube Data API to search channels using `"homefitness"`
   - Extract all channel IDs and metadata

2. **Video Metadata Collection**
   - Use `yt-dlp` to download:
     - Video titles
     - Descriptions
     - Views
     - Video/channel IDs

3. **Language Filtering**
   - Filter out non-English videos using:
     - Language detection via `langdetect` or `scipy`-based heuristics

4. **Manual Labeling with Argilla**
   - Randomly sample a subset of videos
   - Load `title + description` into Argilla
   - Manually label as:
     - ✅ Fitness-related
     - ❌ Not fitness-related

5. **BERT-Based Text Classification**
   - Train a BERT model (e.g., `bert-base-uncased`) to classify videos using the labeled data
   - Input: Concatenated title + description
   - Output: Binary label with confidence score

6. **Automatic Labeling**
   - Use the trained model to classify remaining unlabeled videos
   - Store predictions with confidence thresholds

7. **Topic Modeling with BERTopic**
   - Run BERTopic on **fitness-classified videos**
   - Extract buzzwords and key topics using:
     - Title + description texts
     - Optional: combine with view count for weighting
   - Visualize topic frequency, evolution, and key terms

---

## 🧰 Tech Stack

- **YouTube Data API** – Channel/video search
- **yt-dlp** – Video metadata extraction
- **Argilla** – Manual labeling interface
- **Hugging Face Transformers** – BERT classifier
- **scipy / langdetect** – Language filtering
- **BERTopic** – Topic modeling of classified fitness videos
- **pandas, sklearn, tqdm** – Data processing & ML support

---

## 📁 Project Structure

### Notebooks under `src/` and Order of Execution

1. **`init_db.ipynb`**  
   *Initialize the SQLite database and create all required tables.*

2. **`YT_Channels.ipynb`**  
   *Discover YouTube channels and videos using the YouTube Data API and store metadata in the database.*

3. **`YT_Videos.ipynb`**  
   *Fetch detailed video and channel metadata using `yt-dlp` and update the database.*

4. **`dataset_load.ipynb`**  
   *Load video data from the database, push to Argilla for manual labeling, and manage Argilla datasets.*

5. **`bert_data_prepare.ipynb`**  
   *Prepare labeled data from Argilla for BERT training (cleaning, splitting, etc).*

6. **`bert_classification_train.ipynb`**  
   *Train a BERT model for fitness video classification using the labeled dataset.*

7. **`bertopic_analysis.ipynb`**  
   *Run BERTopic on classified fitness videos to extract and visualize trending topics.*

---

> **Tip:**  
> Run the notebooks in the above order for a smooth end-to-end pipeline execution.

---

