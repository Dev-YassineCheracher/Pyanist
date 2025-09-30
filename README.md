# 🎹 Pyanist: AI Bach Chorale Composer  

Pyanist is a deep learning project that learns the musical style of J.S. Bach's chorales and generates **new, original polyphonic music**.  
It combines **dilated 1D convolutions** (WaveNet-style) with an **LSTM layer** to model both local and long-range harmonic dependencies.  

The project runs entirely in Python and Jupyter Notebook, with interactive MIDI playback using [`music21`](https://web.mit.edu/music21/).  

---

## ✨ Features  

- 🎼 **Data Processing**: Loads Bach chorales from CSV format.  
- 🧠 **Hybrid Model Architecture**: Embedding → Dilated Conv1D stack → LSTM → Dense Softmax.  
- 🎶 **Music Generation**: Produces new chorales note-by-note from a seed sequence.  
- 🎧 **Playback**: Listen directly in the notebook via `music21` MIDI rendering.  

---

## 📂 Dataset  

Uses the **J.S. Bach Chorales** dataset:  
👉 [Kaggle: Bach Chorales](https://www.kaggle.com/datasets/pranjalsriv/bach-chorales-2)  

Expected directory structure:  

```
.
├── dataset/
│   ├── train/
│   │   ├── chorale_000.csv
│   │   ├── ...
│   ├── valid/
│   │   ├── chorale_229.csv
│   │   └── ...
│   └── test/
│       ├── chorale_305.csv
│       └── ...
└── Main.ipynb
```

---

## ⚙️ Installation  

This project uses [`uv`](https://github.com/astral-sh/uv) for fast Python package management.  

1. **Clone Repository**  
```bash
git clone https://github.com/your-username/Pyanist.git
cd Pyanist
```

2. **Set up Environment**  
```bash
uv venv
source .venv/bin/activate   # macOS/Linux
.venv\Scripts\Activate.ps1  # Windows PowerShell
```

3. **Install Dependencies**  
```bash
uv pip install . && uv sync
```

> 💡 Apple Silicon (M1/M2): use `tensorflow-macos` instead of standard `tensorflow` for GPU acceleration.  

---

## 🚀 Usage  

1. **Launch Jupyter**  
```bash
jupyter lab
```

2. **Open Notebook**  
Run through `Main.ipynb` to:  
- Load and inspect chorale data (`pandas`, `music21`).  
- Preprocess sequences (`make_xy` helper).  
- Train the hybrid **Conv + LSTM** model.  
- Generate new chorales with `generate_chorale`.  
- Play results in MIDI format.  

Example generation snippet (from code):  
```python
seed_chords = test_data[3][:8]
new_chorale = generate_chorale(model, seed_chords, length=56)
```

---

## 🏗 Model Architecture  

Sequential hybrid architecture:  

- **Embedding Layer** – maps notes to dense vectors.  
- **Dilated Conv1D Layers** – 5 layers with dilation rates `[1, 2, 4, 8, 16]` + BatchNorm.  
- **LSTM Layer (256 units)** – models long-term dependencies.  
- **Dense Output** – Softmax over possible notes.  

---

## 📊 Training  

Default hyperparameters (see `Main.ipynb`):  

- `window_size = 32`  
- `batch_size = 32`  
- Optimizer: `Nadam(lr=1e-3)`  
- Loss: `sparse_categorical_crossentropy`  
- Epochs: `20`  

---

## 🎵 Example Output  

Generated chorale playback:  

```python
s = stream.Stream()
for row in new_chorale:
    s.append(chord.Chord([n for n in row if n], quarterLength=1))
s.show('midi')
```

This opens a built-in MIDI player in Jupyter to listen to your AI-generated Bach chorale.  

---

## File Structure

```
.
├── Main.ipynb          # Jupyter Notebook with all code for training and generation.
├── README.md           # This file.
├── dataset/            # Contains the Bach chorales dataset (user-provided).
│   ├── test/
│   ├── train/
│   └── valid/
├── pyproject.toml      # Project metadata and dependencies for uv/pip.
└── uv.lock             # Lockfile for reproducible installs with uv.
```
