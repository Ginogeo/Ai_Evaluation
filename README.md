# 🎬 Interactive Wan 2.1 Text-to-Video Generator

This repository contains a Jupyter Notebook implementing an interactive Text-to-Video (T2V) generation pipeline. It utilizes the **Wan 2.1 1.3B** model to generate high-quality video clips from text prompts directly within a notebook environment.

The project features a GUI (Graphical User Interface) built with `ipywidgets`, making it accessible to users without requiring them to modify code for every generation.

## ✨ Features

* **Interactive UI:** Input prompts, select styles, and adjust settings using a visual dashboard (Dropdowns, Sliders, Buttons).
* **Smart Prompt Engineering:** Automatically enhances user inputs by mapping simple style keywords (e.g., "Cinematic") to complex, descriptive prompts (e.g., "cinematic lighting, film grain, dramatic shadows...").
* **Camera Control:** Dedicated controls for camera movement (e.g., "Drone view", "Low angle", "Close-up").
* **Resource Optimized:** * Uses the efficient 1.3B parameter version of Wan 2.1.
    * Implements `bfloat16` precision and `enable_model_cpu_offload()` to run on consumer or cloud GPUs with limited VRAM.
* **Automatic Caching:** Configures a persistent model cache to prevent re-downloading weights across sessions.

## 🛠️ Technical Specifications

* **Model:** `Wan-AI/Wan2.1-T2V-1.3B-Diffusers`
* **Resolution:** 832x480 (16:9 Aspect Ratio)
* **Frame Rate:** 16 FPS
* **Inference:** 30 Steps, Guidance Scale 5.0

## 📦 Requirements

The notebook installs the following dependencies automatically:

* Python 3.10+
* `torch` & `torchvision`
* `diffusers`
* `transformers`
* `accelerate`
* `imageio` & `imageio-ffmpeg`
* `ipywidgets`

## 🚀 Usage

1.  **Launch the Notebook:** Open `video_generator1.ipynb` in Jupyter Lab, Google Colab, or Lightning Studio.
2.  **Run Initialization Cells:** Execute the first few cells to install requirements, load dependencies, and download the model.
    * *Note: The first run may take a few minutes to download the model weights.*
3.  **Scroll to the Interface:** Go to the final cell to see the "Text-to-Video Generator" dashboard.
4.  **Generate:**
    * **Prompt:** Enter your subject and action (e.g., "A cyberpunk detective walking in rain").
    * **Style:** Select a preset (Cinematic, Anime, Vintage, etc.).
    * **Camera:** Choose a viewing angle.
    * **Duration:** Select video length (1s - 10s).
    * **Seed:** Set to `-1` for random, or a specific number for reproducibility.
5.  **View Output:** The generated video will play directly in the notebook and is saved to the `generated_videos` folder.

## ⚙️ Configuration

### Style & Camera Mapping
The notebook uses a dictionary-based mapping system to inject quality boosters into your prompt. You can modify the `STYLE_PROMPTS` and `CAMERA_PROMPTS` dictionaries in the code to add your own presets:

```python
STYLE_PROMPTS = {
    "cinematic": "cinematic lighting, film grain...",
    "anime": "anime style, Japanese animation...",
    # Add your own here
}
