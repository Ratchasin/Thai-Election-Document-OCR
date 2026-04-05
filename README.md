# 🗳️ Thai Election Document OCR (SuperAI Engineer 2569)

An offline OCR pipeline developed to accurately extract political party names and their corresponding voting numbers from scanned Thai election documents. This project was built during the SuperAI Engineer Hackathon.

## 🏆 Performance
* **Metric:** Levenshtein Distance (Lower is better)
* **My Private Score:** `0.9286`
* **Competition Baseline:** `1.0876`
* **Result:** Successfully outperformed the baseline by integrating advanced image preprocessing and heuristic matching algorithms.

## 🛠️ Tech Stack
* **Core OCR:** `EasyOCR` (with GPU acceleration)
* **Computer Vision:** `OpenCV` (cv2) for image preprocessing
* **NLP & String Matching:** `RapidFuzz` (Fuzzy matching), `PyThaiNLP` (Thai numeral conversion)
* **Data Processing:** `Pandas`, `NumPy`, `Dataclasses`

## ⚙️ Data Pipeline & Methodology

Our pipeline is designed to handle noisy document images, skewed alignments, and complex table structures through 4 main steps:

### 1. Image Preprocessing (OpenCV)
Before feeding the images to the OCR engine, we enhanced the image quality:
* **Table Line Removal:** Applied Morphological Operations (`cv2.MORPH_OPEN` with custom horizontal/vertical kernels) to detect and erase table lines, preventing the OCR from misinterpreting lines as characters.
* **Contrast Enhancement:** Utilized CLAHE (Contrast Limited Adaptive Histogram Equalization) to balance lighting.
* **Erosion:** Applied slight erosion to thicken the text for better readability.

### 2. Text Extraction (EasyOCR)
* Passed the preprocessed images into EasyOCR to extract text and bounding boxes.
* Encapsulated the output into a structured `TextBox` Dataclass containing `raw_text`, `x_center`, `y_center`, and `box_height`.

### 3. Heuristic Matching & Post-Processing
Since OCR outputs are unstructured, we used a spatial heuristic approach:
* **Fuzzy Matching:** Used `RapidFuzz` to match noisy OCR text (e.g., misspelled party names) with the exact expected party names.
* **Coordinate Mapping:** Mapped vote numbers to parties by calculating the absolute difference in `y_center` (row alignment) and ensuring the number's `x_center` is to the right of the party name.
* **Number Cleansing:** Converted Thai numerals to Arabic numerals using `PyThaiNLP` and cleaned noise characters (e.g., replacing 'O' with '0').

## 🚀 How to Run
1. Ensure you have the required libraries installed:
   ```bash
   pip install easyocr pythainlp python-Levenshtein pandas tqdm rapidfuzz opencv-python-headless
