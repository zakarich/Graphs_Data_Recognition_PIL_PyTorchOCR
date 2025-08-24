# Graphs Data Recognition (PIL + PyTorch OCR)

Tools and experiments for recognizing **text and structure from chart images** (e.g., axis labels, tick marks, legends, titles) using **Python, PIL**, and **PyTorch-based OCR**—in the context of the **Benetech – Making Graphs Accessible** task (chart transcription to accessible formats).

> Repo snapshot shows a notebook for plotting annotations with PIL and a working area for the Benetech dataset: `plot_ann_using_pil.ipynb` and a folder `benetech-making-graphs-accessible/`. :contentReference[oaicite:0]{index=0}

---

## 🚀 What’s inside

- **Annotation Visualizer (PIL/Jupyter)**  
  Inspect and overlay chart annotations (bounding boxes, labels) to verify dataset quality and guide OCR pre/post-processing. *(See `plot_ann_using_pil.ipynb`.)* :contentReference[oaicite:1]{index=1}

- **Benetech graph-understanding workspace**  
  Folder placeholder for data, scratch notebooks, and scripts related to the **Benetech – Making Graphs Accessible** problem (bar/line/scatter charts). :contentReference[oaicite:2]{index=2}

- **OCR pipeline scaffolding (WIP)**  
  Designed to plug in PyTorch OCR (e.g., CRNN/CTC) or 3rd-party OCR backends (pytesseract/MMOCR/PaddleOCR alternatives). Includes preprocessing utilities (resize, binarize, deskew, crop by detected ROIs).

---

## 🗺 Planned pipeline (high level)

Data ingest

Load Benetech training/validation images + annotations (charts & text regions)

Preprocess

PIL-based image cleanup (resize, grayscale, denoise, contrast)

Optional geometric ops (deskew, margin crop)

Region of Interest (ROI) detection

Start simple: use provided boxes from annotations (visualize in the notebook)

Upgrade path: add detector (YOLOv7/EfficientDet) to locate text/axes/legend
and feed crops to OCR for robust performance

OCR & text parsing

PyTorch OCR model (CRNN/CTC) or external OCR

Normalize outputs, strip artifacts, unify number formats

Structural reconstruction

Associate text with axes/ticks/legend entries

Map values back to data series for CSV/JSON export

Evaluation

Local metrics mirroring Benetech challenge scoring

Qualitative checks with the annotation visualizer


> Benetech task background & dataset details: competition overview and data pages. :contentReference[oaicite:3]{index=3}  
> Many strong solutions used a **detector → OCR** two-stage approach; see public discussions/notebooks for context. :contentReference[oaicite:4]{index=4}

---

## 🧱 Repo structure

Graphs_Data_Recognition_PIL_PyTorchOCR/
├─ benetech-making-graphs-accessible/ # workspace for data/scripts (add your data here)
├─ plot_ann_using_pil.ipynb # annotate/visualize chart boxes & labels (PIL)
└─ .ipynb_checkpoints/ # Jupyter auto-checkpoints


*Files/folders as visible on GitHub at the time of writing.* :contentReference[oaicite:5]{index=5}

---

## 🛠 Tech stack

| Area            | Tools / Libraries (suggested) |
|-----------------|-------------------------------|
| Core            | Python 3.9+                   |
| Imaging         | Pillow (PIL), OpenCV (optional) |
| OCR             | PyTorch (+ CRNN/CTC) or alternatives (pytesseract/MMOCR/PaddleOCR) |
| DS/Utils        | NumPy, Pandas, Matplotlib     |
| Notebooks       | Jupyter / JupyterLab          |

> Swap OCR backends as needed; the pipeline is backend-agnostic. For chart element detection upgrades, try YOLOv7/EfficientDet to crop robust ROIs before OCR. :contentReference[oaicite:6]{index=6}

---

## ⚡ Getting started

### 1) Clone
git clone https://github.com/zakarich/Graphs_Data_Recognition_PIL_PyTorchOCR.git
cd Graphs_Data_Recognition_PIL_PyTorchOCR


### 2) (Optional) Create a virtual environment
python -m venv .venv

Windows
.venv\Scripts\activate

macOS / Linux
source .venv/bin/activate

### 3) Install dependencies
pip install -U pip
pip install pillow numpy pandas matplotlib torch torchvision torchaudio

Optional OCR backends / extras:
pip install pytesseract opencv-python mmocr easyocr shapely


### 4) Get the data
Download the Benetech – Making Graphs Accessible dataset from Kaggle
and place images/annotations under: benetech-making-graphs-accessible/
(See Kaggle competition pages for dataset and rules.)

*Competition overview & data pages for reference.* :contentReference[oaicite:7]{index=7}

### 5) Run the annotation visualizer
jupyter notebook

Open: plot_ann_using_pil.ipynb
Configure paths in the first cells to point to your data


---

## 🧪 Example usage (pseudo)

```python
from PIL import Image
import numpy as np

# 1) Load image
im = Image.open("benetech-making-graphs-accessible/train/images/0001.png")

# 2) Preprocess (example)
im = im.convert("L")            # grayscale
im = im.point(lambda x: 0 if x < 180 else 255)  # simple threshold

# 3) Crop ROIs from provided boxes (demo coords)
x, y, w, h = 100, 240, 640, 64
roi = im.crop((x, y, x + w, y + h))

# 4) Pass ROI to your OCR model backend (placeholder)
# text = ocr_infer(roi)
# print(text) 
```

## 📈 Roadmap
- Add a unified config + CLI (preprocess → detect → OCR → export)
- Plug in a detector (YOLOv7/EfficientDet) for chart components
- Train/finetune CRNN (or use MMOCR/PaddleOCR) for stronger text recognition
- Build alignment logic (associate ticks/labels → numeric scale)
- Export to CSV/JSON; optional Kaggle-style evaluation scripts
- Add tests and CI; publish example results

## 🤝 Contribution

Contributions are welcome!

1) Fork the repo
2) Create a feature branch
3) Commit with clear messages
4) Open a PR describing the approach & results
5) If adding a backend/detector, include a small demo in a notebook


## 📬 Contact

Maintainer: @zakarich  (https://github.com/zakarich)
Open an issue for questions/ideas or reach out for collaboration.

## 📄 License

MIT License

Copyright (c) 2025 zak

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the “Software”), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES
OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT
HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY,
WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER
DEALINGS IN THE SOFTWARE.


## 🔖 References & context

Benetech – Making Graphs Accessible (overview & data)
- Competition overview: Kaggle
- Dataset page: Kaggle
Two-stage detector→OCR discussions & notebooks
- Example discussion/notebook patterns: Kaggle
