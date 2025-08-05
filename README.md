# 🟪 Polygon Coloring with UNet

This project implements a deep learning model based on **UNet** to automatically color polygon shapes (like triangles, squares, hexagons) based on a given color name. It uses a trained segmentation model and inference pipeline to output clean, solid-colored polygon images with white backgrounds.

---

## 🧠 Model

- **Architecture**: UNet
- **Input**: Polygon image (`.png`)
- **Conditioning**: One-hot encoded color name
- **Output**: Image of the polygon filled with the specified solid color
