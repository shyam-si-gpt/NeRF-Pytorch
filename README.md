# NeRF with PyTorch (Blender and Custom LLFF Data Support)

This is a PyTorch implementation of **Neural Radiance Fields (NeRF)** for novel view synthesis using both Blender synthetic scenes and custom real-world data.

---

## 🛠️ Installation

```bash
git clone <your_repo_link_here>
cd nerf-pytorch
pip install -r requirements.txt
```

**Required Libraries:**
- PyTorch 1.4+
- matplotlib
- numpy
- imageio
- imageio-ffmpeg
- configargparse
- COLMAP (for custom datasets)

---

## 🚀 Running NeRF

### 1. Blender Synthetic Datasets

1. Download example Blender datasets:
   ```bash
   bash download_example_data.sh
   ```

2. Train NeRF on a Blender scene like **lego**:
   ```bash
   python run_nerf.py --config configs/lego.txt
   ```

Outputs will be saved in `logs/lego_test/`.

---

### 2. Custom LLFF-style Real-world Data

#### 📷 Step-by-step Pipeline

1. **Organize your images:**
   ```
   your_scene/
   └── images/         # Your real-world input photos
   ```

2. **Run COLMAP**  
You can use either **GUI** or **CLI**:
- **GUI Option**:
  - Launch `colmap`
  - Run `Feature Extraction`
  - Run `Exhaustive Matching`
  - Run `Mapper` (choose `images` as input, output to `sparse`)
- **CLI Option**:
   ```bash
   colmap feature_extractor --database_path your_scene/database.db --image_path your_scene/images
   colmap exhaustive_matcher --database_path your_scene/database.db
   mkdir your_scene/sparse
   colmap mapper --database_path your_scene/database.db --image_path your_scene/images --output_path your_scene/sparse
   ```

3. **Convert COLMAP output using imgs2poses.py:**
   ```bash
   python imgs2poses.py --match_type exhaustive_matcher your_scene/
   ```

   This generates:
   ```
   your_scene/
   ├── images/
   ├── sparse/0/
   ├── poses_bounds.npy ✅
   ```

4. **Train NeRF on your custom scene:**
   ```bash
   python run_nerf.py --config configs/fern.txt --datadir ./your_scene --dataset_type llff
   ```

5. **(Optional) Render only:**
   ```bash
   python run_nerf.py --config configs/fern.txt --render_only --datadir ./your_scene --dataset_type llff
   ```

---

## 🔄 For 360-Degree Spherical Scenes

If your data is captured in a full loop (360°), use additional flags:

```bash
python run_nerf.py --config configs/fern.txt --datadir ./your_scene --dataset_type llff --no_ndc --spherify --lindisp
```

### Summary of Optional Flags

| Flag         | Purpose                                                                 |
|--------------|-------------------------------------------------------------------------|
| `--no_ndc`   | Disables NDC (normalized device coords), helpful for real-world data    |
| `--spherify` | Rearranges poses for spherical scenes                                   |
| `--lindisp`  | Uses linear disparity (useful for wide baseline scenes)                 |

---

## 📂 Project Directory Structure

```
├── configs/
│   ├── lego.txt (Blender dataset config)
│   ├── fern.txt (LLFF-style real-world dataset config)
│
├── data/
│   ├── nerf_synthetic/
│   │   ├── lego/
│   ├── nerf_llff_data/
│   │   ├── your_custom_scene/
│
├── run_nerf.py     # Main training and rendering script
├── imgs2poses.py   # Converts COLMAP output to poses_bounds.npy
```

---

## 🧭 Summary: Custom Data Workflow

| Step | Tool / Script          | Required |
|------|------------------------|----------|
| Image Collection | Manual                  | ✅ |
| COLMAP SfM       | COLMAP GUI/CLI          | ✅ |
| Pose Conversion  | `imgs2poses.py`         | ✅ |
| Train NeRF       | `run_nerf.py`           | ✅ |

---

# 🔥 You’re now ready to train NeRF on both Blender and your own real-world data 🔥
