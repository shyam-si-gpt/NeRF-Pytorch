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

### 2. Running NeRF on Custom Real-world Data

For your own captured images:

1. **Preprocess with COLMAP**:
   - Use [COLMAP](https://colmap.github.io/) to reconstruct the scene and estimate camera poses.
   - Then use `imgs2poses.py` to generate the required `poses_bounds.npy`.

2. **Organize the data** as:
   ```
   data/
     nerf_llff_data/
       your_custom_scene/
         images/          # All input images
         poses_bounds.npy # Generated from imgs2poses.py
   ```

3. **Train NeRF** on your dataset:
   ```bash
   python run_nerf.py --config configs/fern.txt --datadir ./data/nerf_llff_data/your_custom_scene
   ```

4. (Optional) To render only:
   ```bash
   python run_nerf.py --config configs/fern.txt --render_only --datadir ./data/nerf_llff_data/your_custom_scene
   ```

---

## Project Structure

```
├── configs/
│   ├── lego.txt (Blender dataset config)
│   ├── fern.txt (Real-world dataset config)
│
├── data/
│   ├── nerf_synthetic/
│   │   ├── lego/
│   ├── nerf_llff_data/
│   │   ├── your_custom_scene/
│
├── run_nerf.py    # Main training and testing script
```

---

## Quick Reference Table

| Dataset Type           | Preprocessing Needed       | Run Command |
|-------------------------|-----------------------------|-------------|
| Blender synthetic data  | No                          | `python run_nerf.py --config configs/lego.txt` |
| Custom real-world data  | COLMAP + imgs2poses.py       | `python run_nerf.py --config configs/fern.txt --datadir ./data/nerf_llff_data/your_custom_scene` |

---

## Notes

- For Blender datasets, poses are already known.
- For real-world captures, accurate COLMAP reconstruction is necessary for good results.
- Make sure your custom images are high-quality and have sufficient overlap.

---

# 🔥 That's it! Ready to train NeRF on both Blender and your own custom data 🔥
