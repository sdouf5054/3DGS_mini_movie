# 3DGS_mini_movie
For CV Final Project: Mini-Movie Making with dynamic 3DGS

This repository contains the full implementation pipeline for a short dynamic mini-movie created using 3D Gaussian Splatting.  
The final output is an **11-second rendered animation** that integrates **camera motion** and **object motion** in a reconstructed 3D scene.

---

## 🎬 Final Output

https://github.com/user-attachments/assets/fa0935b3-80a4-4326-9435-0f330a319963

- **Video**: `[mini-movie]fever_dream_of_declined.mp4`
- **Length**: 11 seconds (330 frames @ 30 FPS)

### Concept
The mini-movie reconstructs a dream sequence of a student who, after being rejected from graduate school, drinks whiskey and falls asleep  
(*never based on a true story*).

In this dreamlike narrative:
- A **Kermit the Frog plush** beside the bed becomes animated and gradually enlarged.
- A **whiskey bottle** moves, rotates, and oscillates in sync with the scene.
- The animation visually represents intoxication, exaggeration, and distorted self-perception.

---

## Notebook Overview

The implementation consists of **five Jupyter notebooks**, located in the `notebooks/` directory.

> **Important Note on Reproducibility**  
> Due to time constraints, these notebooks:
> - lack full documentation,
> - contain inconsistent comments,
> - use environment- and machine-specific file paths.  
>
> **Direct execution without modification is unlikely to succeed.**  
> They are provided primarily to demonstrate **implementation logic and workflow**, not as a turnkey pipeline.

### 1. `01_prepare_frames_for_colmap.ipynb`
- Extracts frames from scene videos (e.g., `.mov`).
- Parameters include start frame and frame step (e.g., every 6 frames).
- Outputs are used for:
  - COLMAP point cloud reconstruction
  - 3DGS training inputs
- Initially handled both scene and objects, later separated due to segmentation requirements.

---

### 2. `02_[COLAB]_SAM3_Production_Official_API_2.ipynb`
- Object segmentation using **SAM3 (Segment Anything Model 3)** by Meta.
- Requires Hugging Face access approval.
- Text-prompt-based segmentation:
  - `"Kermit"` (threshold 0.4)
  - `"a whiskey bottle"` (threshold 0.2)
- Implemented **exclusively for Google Colab** due to model size and compute requirements.

---

### COLMAP Processing (External)
- Performed locally using **COLMAP GUI (Windows)**.
- Camera model:
  - Initially `SIMPLE_RADIAL`
  - Changed to `PINHOLE` for proper 3DGS training compatibility
- This step follows the same workflow as previous assignments.

---

### 3. `03_[COLAB]_3DGaussianSplatting_INRIA_Method_Colab.ipynb`
- 3DGS training notebook.
- Based on a modified INRIA repository addressing:
  - CUDA
  - PyTorch
  - Colab compatibility issues
- Local training failed even at 1/8 resolution for the scene.
- Colab-based training was adopted to preserve visual quality.
- Intermediate PLY files were inspected using an online Gaussian Splat viewer.

---

### 4. `04_storyboard_implementation.ipynb`
- **Core notebook of the project.**
- Responsibilities:
  - Align scene and object PLYs into a unified coordinate system
  - Enforce Z-axis as vertical direction
  - Generate trajectory JSON (camera + object motion)
- The storyboard was iteratively refined; comments may reflect earlier design versions.

---

### 5. `05_[COLAB]_render_minimovie_COLAB.ipynb`
- Renders the final video using:
  - Aligned PLY files
  - Trajectory JSON from the storyboard notebook
- Designed for **Google Colab execution**.
- Output was post-processed by rotating 90° to obtain portrait orientation.

