# NeRF with PyTorch (Blender and Custom Data Support)

This is a PyTorch implementation of **Neural Radiance Fields (NeRF)** for novel view synthesis.

This repository supports:
- Running NeRF directly on **Blender synthetic datasets**.
- Running NeRF on **custom real-world datasets** using **COLMAP** preprocessing.

---

## Installation

```bash
git clone <your_repo_link_here>
cd nerf-pytorch
pip install -r requirements.txt
```

**Required Libraries:**
- PyTorch
- matplotlib
- numpy
- imageio
- imageio-ffmpeg
- configargparse
- COLMAP (for custom datasets)

---

## How to Run

### 1. Running NeRF on Blender Datasets

1. Download example Blender datasets:
   ```bash
   bash download_example_data.sh
   ```

2. Train NeRF on a Blender scene like **lego**:
   ```bash
   python run_nerf.py --config configs/lego.txt
   ```

After training, the novel view renderings will be saved in the `logs/lego_test/` folder.

---
