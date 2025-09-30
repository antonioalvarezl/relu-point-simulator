# Neural ODE Point Flow Simulator

Interactive demo to explore the evolution of **point clouds** under a single-unit ReLU Neural ODE, with **multi-step composition**, **free / interpolation / classification** modes, and simple **separability** and **cross-entropy** metrics.

**Live:** https://antonioalvarezl.github.io/relu-point-simulator/  
**Code:** `index.html` (single file; see below)  
**License:** MIT

## Inspiration

This simulator is inspired by recent work on Neural ODEs viewed through control and architectural complexity:

- **Álvarez-López, Orive-Illera & Zuazua (2025)**. *Cluster-Based Classification with Neural ODEs via Control*. **Journal of Machine Learning**, 4(2), 128–156. https://doi.org/10.4208/jml.241114  
- **Álvarez-López, Hadj Slimane & Zuazua (2024)**. *Interplay between depth and width for interpolation in neural ODEs*. **Neural Networks**, 180, 106640. https://doi.org/10.1016/j.neunet.2024.106640  
- **Ruiz-Balet & Zuazua (2023)**. *Neural ODE Control for Classification, Approximation, and Transport*. **SIAM Review**, 65(3), 735–773. https://doi.org/10.1137/21M1411433

## Overview

We simulate the continuous-time model
\[
\dot x(t)= w\,\big(a^\top x(t)+b\big)_+,
\]
with \(x\in\mathbb{R}^2\) and per-step, piecewise-constant parameters \((w,a,b)\). The full map is the **composition** across steps. Integration uses explicit Euler with 50 sub-steps per segment.

**Modes**
- **Free:** inspect the vector field and trajectories.
- **Interpolation:** track average point-to-target distance.
- **Classification:** drive classes into **horizontal bands** (separability) or minimize a simple **cross-entropy** built from band-center logits.

## Quick start

- **Online:** open the *Live* link when available.  
- **Local:** clone/download and open `index.html` in your browser. No build needed.

## Controls & Options

- **Mode:** Free / Interpolation / Classification.  
- **Population:** number of points (1–500); uniform or per-class Gaussian sampling.  
- **Multi-step timeline:** add/remove steps; each step has sliders/inputs for \(w_1,w_2,a_1,a_2,b\).  
- **Global time:** slider \([0,1]\) with **Play/Pause** and **Reset**; time is split evenly across steps.  
- **Visualization:** point size; vector-field density/opacity; activation hyperplane \(a^\top x+b=0\).  
- **Interaction:** mouse wheel to zoom; drag to pan; save **PNG** or record a **WebM** of the evolution.

The **metrics** panel shows:
- **Interpolation:** mean distance to targets.  
- **Classification:**  
  - *Separability*: share of points inside their class band \(y\in[\frac{k}{M},\frac{k+1}{M})\).  
  - *Cross-entropy*: softmax loss using logits \(-|y-y_c|\) with band centers \(y_c=(k+\tfrac12)/M\).

## Mathematical background (per-step flow)

With fixed \((w,a,b)\) in a step, the ODE is **piecewise-affine** with respect to the half-spaces \(H^+=\{x:\ a^\top x+b>0\}\) and \(H^-=\{x:\ a^\top x+b\le0\}\).
- In \(H^-\): the field vanishes and particles **do not move**.  
- In \(H^+\): the field points along \(w\) with magnitude \(\max(0,a^\top x+b)\). The simulator integrates trajectories by Euler (50 sub-steps over the fractional time of that segment).

In **classification by separability**, the final \(y\) coordinate is compared with equispaced horizontal bands. In **cross-entropy**, logits come from vertical proximity to band centers.

## UI notes

- **Canvas:** scatter plot (color by class), shaded inactive region \(a^\top x+b<0\), and vector-field arrows.  
- **Steps panel:** tabbed *Step i* sections with synchronized sliders and numeric inputs.  
- **Recording:** exports **WebM** (the UI shows an `ffmpeg` command to convert to MP4).  
- **Performance:** high point counts or dense vector fields can reduce FPS; tune density/opacity.

## Educational uses

- **Students:** build intuition about ReLU gating and composed continuous flows.  
- **Researchers:** quick checks of active/inactive regimes and width–depth trade-offs in interpolation/classification.  
- **Instructors:** stage multi-step examples and switch to different metrics for clarity.

## Code (single file)

Everything lives in one HTML file (UI + canvas + logic), using the 2D Canvas API, `MediaRecorder` for video, and a simple Euler integrator. Use the `index.html` provided in this repository (the file included in this project) and open it directly in a browser.

## Citation

If this demo supports your teaching or research, please cite:

```bibtex
@article{AlvarezLopez2025ClusterNODE,
  author  = {Álvarez-López, Antonio and Orive-Illera, Rafael and Zuazua, Enrique},
  title   = {Cluster-Based Classification with Neural ODEs via Control},
  journal = {Journal of Machine Learning},
  volume  = {4},
  number  = {2},
  pages   = {128--156},
  year    = {2025},
  doi     = {10.4208/jml.241114},
  url     = {https://global-sci.com/article/91926/cluster-based-classification-with-neural-odes-via-control}
}

@article{AlvarezLopez2024DepthWidthNODE,
  author  = {Álvarez-López, Antonio and Hadj Slimane, Arselane and Zuazua, Enrique},
  title   = {Interplay between depth and width for interpolation in neural ODEs},
  journal = {Neural Networks},
  volume  = {180},
  pages   = {106640},
  year    = {2024},
  doi     = {10.1016/j.neunet.2024.106640},
  url     = {https://doi.org/10.1016/j.neunet.2024.106640}
}

@article{RuizBaletZuazua2023NODEControl,
  author  = {Ruiz-Balet, Dom\`enec and Zuazua, Enrique},
  title   = {Neural ODE Control for Classification, Approximation, and Transport},
  journal = {SIAM Review},
  volume  = {65},
  number  = {3},
  pages   = {735--773},
  year    = {2023},
  doi     = {10.1137/21M1411433},
  url     = {https://epubs.siam.org/doi/10.1137/21M1411433}
}
