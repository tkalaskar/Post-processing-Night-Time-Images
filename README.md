# Post-processing Night-Time Images

A MATLAB implementation of an **illumination boosting algorithm** for enhancing the visibility and contrast of night-time photographs — brightening dark regions, improving local contrast, and normalizing the result, without the need for a reference/well-lit image.

The technique is based on:

> Al-Ameen, Zohair. *"Nighttime image enhancement using a new illumination boost algorithm."*

## How it works

The core function, `illumination_boost(X, lambda)`, enhances an RGB image in five stages:

| Stage | What it does |
|---|---|
| **I1** | Logarithmic scaling of pixel intensities |
| **I2** | A non-complex exponential transform that softens (attenuates) high-intensity regions and adjusts local contrast |
| **I3** | Combines I1 and I2 using a LIP (Logarithmic Image Processing) model, weighted by `lambda` |
| **I4** | A modified CDF-HSD function (using `erf` and `atan`) that boosts brightness specifically in dark regions |
| **I5** | Normalizes I4 back into a displayable range — this is the final enhanced image |

`lambda` controls the strength/behavior of the LIP blending step — higher values generally push the enhancement further. The scripts in this repo sweep `lambda` across a small range to compare results.

## Repository contents

| File / Folder | Description |
|---|---|
| `illumination_boost.m` | Core function implementing the enhancement algorithm. Takes an RGB image and a `lambda` value, returns the enhanced image. |
| `test_illumination_boost.m` | Batch script: runs every `.jpg` in `input_images/` through `illumination_boost` for `lambda = 2..6`, plots the original alongside all 5 enhanced versions, and saves the figure (as `.jpg` and `.fig`) to `output_images/`. |
| `plot_with_histogram.m` | Demo script: enhances a single sample image (`city_at_night.jpg`, `lambda = 1.5`) and plots the original vs. enhanced image **with their pixel-intensity histograms** side by side. |
| `input_images/` | Sample night-time photos used as input. |
| `output_images/` | Generated comparison figures (original vs. enhanced at different lambda values). |

## Requirements

- MATLAB (any reasonably recent version)
- Image Processing Toolbox (for `imread`, `imshow`, `im2double`, `histogram`)

## Usage

**Run the full batch comparison** (all images in `input_images/`, multiple lambda values):

```matlab
test_illumination_boost
```
This produces, for each input image, a 2×3 grid: the original image plus 5 enhanced versions (`lambda = 2` through `6`), saved to `output_images/`.

**Run the single-image histogram demo:**

```matlab
plot_with_histogram
```
This loads `input_images/city_at_night.jpg`, enhances it with `lambda = 1.5`, and displays original/enhanced images next to their histograms.

**Use the function directly on your own image:**

```matlab
I = imread('your_image.jpg');
enhanced = illumination_boost(I, 3);
imshow(enhanced);
```

## Output

Running `test_illumination_boost` populates `output_images/` with one comparison figure per input photo (saved both as `.jpg` for quick viewing and `.fig` for re-opening/editing in MATLAB).
