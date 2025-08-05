# 🧠 Polygon Colorization with Color-Conditioned UNet

This project implements a UNet-based deep learning model to **color polygon shapes** with a **target color** using **vector-based conditioning**. The primary objective is to map grayscale polygon outlines to solidly filled versions based on a given RGB color prompt, maintaining a clean white background and sharp edges.

---

## 📐 Architecture Overview

- **Model**: Modified UNet
- **Conditioning Strategy**: Color vector (RGB or one-hot label) is **broadcast and concatenated** into the encoder bottleneck before the decoder pathway.
- **Input**: Grayscale or outline polygon image (`3×H×W`) + a **target color vector** (e.g., `[1, 0, 0]` for red).
- **Output**: Colorized polygon image with:
  - Target polygon filled with the correct solid color.
  - Background completely white.
- **Output Activation**: `Sigmoid` to scale output to [0, 1] for pixel-wise regression.
- **Loss Function**: `MSELoss` for regression towards exact RGB output.
- **Postprocessing**:
  - Color snapping: Nearest color from palette.
  - Background filtering using mask threshold.

---

## 🔧 Hyperparameters

| **Parameter**     | **Values Tried**            | **Final Value** | **Why?** |
|------------------|-----------------------------|-----------------|----------|
| Optimizer        | Adam, AdamW                 | Adam            | Stable and fast convergence |
| Learning Rate    | 1e-2, 1e-3, 5e-4, 1e-4       | 1e-3            | Fast start, no overshooting |
| Epochs           | 20, 50, 100                 | 50              | Reached convergence, stable outputs |
| Batch Size       | 4, 8, 16                    | 8               | Balanced between speed & memory |
| Image Size       | 128×128, 256×256            | 128×128         | Smaller sizes sufficient |
| Loss             | MSE, L1, BCE                | MSE             | Pixel-wise fidelity preferred |
| Augmentations    | Flips, Noise, Brightness    | Horizontal Flip | Improved generalization |

---

## 📈 Training Dynamics

### 📉 Loss Curves
- **Training Loss**: Gradual descent, stabilizes by ~40 epochs.
- **Validation Loss**: Follows closely, indicating minimal overfitting.

### 🔎 Metric Trends
- Dice coefficient & Intersection over Union (IoU) were computed in some versions for edge clarity.
- Pixel-wise MSE was the primary target for regression to RGB.

---

## 🧪 Qualitative Results

| Input | Target Color | Output |
|-------|---------------|--------|
| ![polygon](...) | Red (`[255, 0, 0]`) | ![red_fill](...) |
| ![polygon](...) | Green (`[0, 255, 0]`) | ![green_fill](...) |

- Outputs matched desired RGB values post-snapping.
- Background preserved as white using mask filtering.
- Sharp polygon boundaries achieved via UNet skip connections.

---

## ⚠️ Failure Cases & Fixes

| Problem | Fix |
|--------|-----|
| Output had soft shades instead of solid color | Used vector snapping to closest palette |
| Background had grey tint | Applied binary mask threshold to clean background |
| Colors close but not exact (e.g., `[252, 4, 6]` instead of `[255, 0, 0]`) | Rounded to nearest color via cosine similarity |
| Jagged edges | Used UNet's skip connections effectively + post-mask filtering |

---

## 🧠 Key Learnings

- **Conditioning networks** with non-image data (like color vectors) is powerful and controllable.
- A **small UNet** (4-level) is enough for simple synthetic tasks.
- **Preprocessing & postprocessing** (e.g., mask filtering, snapping) often matter as much as the model itself.
- **Training on synthetic data** can yield sharp results when your pipeline is clean and targets are deterministic.

---


## 🛠️ Setup Instructions

```bash
# Clone the code
https://github.com/ajaytelukutla19/polygon_project/blob/main/polygon_shape.ipynb


# Install dependencies
pip install -r requirements.txt

# Train the model
python train.py --epochs 50

# Run inference
python inference.py --image input.png --color red
```

---

## 📬 Contact

For questions or collaboration, feel free to reach out via [GitHub Issues](https://github.com/ajaytelukutla19).

---
