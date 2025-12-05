# Video Generator Pipeline Report

## 1. Pipeline Design

### Architecture Overview
The pipeline follows a simple three-stage architecture:

```
User Input → Prompt Construction → Video Generation → Output
```

**Stage 1: Input Collection**
- Interactive widgets collect user parameters (subject/action, style, camera angle, duration)
- Input validation ensures parameters are within acceptable ranges

**Stage 2: Prompt Construction**
- User inputs are enhanced with style-specific and camera-specific descriptors
- A composite prompt is built: `{subject_action}, {camera_desc}, {style_desc}, high quality, smooth motion`
- Negative prompts filter undesirable artifacts

**Stage 3: Video Generation**
- The Wan 2.1 model generates video frames
- Frames are exported to MP4 format at 16 FPS
- Memory cleanup ensures efficient resource usage

### Key Components
| Component | Purpose |
|-----------|---------|
| `construct_prompt()` | Builds enhanced prompts from user inputs |
| `duration_to_frames()` | Converts time to valid frame counts |
| `generate_video()` | Main generation pipeline |
| `evaluate_video()` | Qualitative assessment helper |

---

## 2. Model Selection: Wan 2.1 T2V

### Why Wan 2.1?
- **Open Source**: Freely available on Hugging Face with Apache 2.0 license
- **Quality**: State-of-the-art text-to-video generation with coherent motion
- **Efficiency**: 1.3B parameter version balances quality and compute requirements
- **Integration**: Native Diffusers library support enables simple integration
- **FP16/BF16 Support**: Memory-efficient inference without significant quality loss

### Model Configuration
```python
MODEL_ID = "Wan-AI/Wan2.1-T2V-1.3B-Diffusers"
Resolution: 480 × 832
Frame Rate: 16 FPS
Inference Steps: 30
Guidance Scale: 5.0
```

### Memory Optimization
- CPU offloading enabled via `enable_model_cpu_offload()`
- BFloat16 precision for transformer components
- Explicit garbage collection after generation

---

## 3. Parameter Handling Strategy

### Text Prompt Enhancement
User prompts are augmented with contextual descriptors:

**Style Mappings:**
| Style | Added Descriptors |
|-------|-------------------|
| Cinematic | cinematic lighting, film grain, dramatic shadows |
| Cartoon | cartoon style, vibrant colors, hand-drawn aesthetic |
| Realistic | photorealistic, 4K quality, natural lighting |
| Anime | anime style, cel shading, expressive |
| Vintage | vintage film, retro aesthetic, faded colors |

**Camera Mappings:**
| Camera | Added Descriptors |
|--------|-------------------|
| Front View | front view, facing camera |
| Side View | side view, profile shot |
| Top-Down | bird's eye perspective, overhead shot |
| Low Angle | looking up, dramatic perspective |
| Close-Up | close-up shot, detailed focus |
| Wide Shot | establishing shot, full scene view |

### Duration Handling
- Wan 2.1 requires frame counts following the 4k+1 pattern
- Duration is converted to the nearest valid frame count
- Valid options: 17, 25, 33, 41, 49, 57, 65, 81 frames
- Maximum ~5 seconds at 16 FPS to balance quality and generation time

### Seed Management
- Optional seed input for reproducibility
- Random seed generated if -1 is provided
- Seed is logged for result reproduction

---

## 4. Evaluation and Observations

### Evaluation Framework
Videos are assessed on five criteria (1-5 scale):

1. **Prompt Adherence**: Does the subject perform the described action?
2. **Style Accuracy**: Is the visual style correctly applied?
3. **Camera Angle**: Does the perspective match the request?
4. **Motion Quality**: Is movement smooth and natural?
5. **Visual Quality**: Overall clarity and coherence

### Expected Observations

**Strengths:**
- Good text-to-motion understanding for common actions
- Consistent subject identity across frames
- Style prompts effectively influence visual aesthetics
- Smooth temporal coherence in short clips

**Limitations:**
- Complex multi-subject scenes may have artifacts
- Very specific camera movements may not be precisely followed
- Style transfer varies in effectiveness across different styles
- Longer durations (>3s) may show quality degradation

### Sample Generation Results

| Prompt | Style | Camera | Duration | Quality Notes |
|--------|-------|--------|----------|---------------|
| Cat playing with ball | Cartoon | Front | 2s | Good subject, clear motion |
| Bird flying over mountains | Cinematic | Wide | 2s | Scenic, smooth flight path |
| Person walking | Realistic | Side | 3s | Natural gait, good perspective |

---

## 5. Conclusion

This pipeline provides a minimal yet functional interface for text-to-video generation using the Wan 2.1 model. The prompt enhancement strategy effectively translates user intentions into model-friendly inputs, while the evaluation framework enables systematic quality assessment. The implementation stays under 200 lines while providing full customization capabilities.

### Future Improvements
- Add image-to-video capability
- Implement batch generation
- Add video upscaling post-processing
- Enable custom style fine-tuning
