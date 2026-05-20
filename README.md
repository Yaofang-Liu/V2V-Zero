<div align="center">

# V2V-Zero

### Beyond Text Prompts: Visual-to-Visual Generation as A Unified Paradigm

[![arXiv](https://img.shields.io/badge/arXiv-2605.12271-b31b1b.svg?style=flat-square)](https://arxiv.org/abs/2605.12271)
[![Project Page](https://img.shields.io/badge/Project-Page-1f6feb.svg?style=flat-square)](https://yaofang-liu.github.io/V2V_Web/)
[![License](https://img.shields.io/badge/License-MIT-green.svg?style=flat-square)](LICENSE)
[![Code](https://img.shields.io/badge/Code-Coming_Soon-orange.svg?style=flat-square)](#release-roadmap)
[![Benchmark](https://img.shields.io/badge/Simple--V2V_Bench-Coming_Soon-orange.svg?style=flat-square)](#release-roadmap)

<p align="center">
  <a href="https://yaofang-liu.github.io/V2V_Web/">
    <img src="https://yaofang-liu.github.io/V2V_Web/assets/figures/fig_main_visual_prompting.png" width="92%" alt="V2V-Zero Method Overview"/>
  </a>
</p>

**A frozen VLM. A frozen DiT. A visual page between them.**

</div>

---

## TL;DR

We propose **Visual-to-Visual (V2V) generation** — users condition generators with a *visual specification page* instead of a text prompt. **V2V-Zero** is a **training-free** instantiation: it routes the visual page through a frozen VLM into a frozen diffusion generator's existing conditioning interface.

- **0.85** on GenEval with frozen Qwen-Image (vs. 0.87 official optimized T2I).
- **95.0%** of DiT conditioning attention is routed through visual-page hidden states.
- **Simple-V2V Bench**: 7 tasks × 7 models × 154 prompts — first benchmark for visual conditioning.
- A **HunyuanVideo-1.5** extension confirms V2V transfers beyond images.

> 🌐 **Explore the project page** for visualizations, baseline comparisons, and video demos: **<https://yaofang-liu.github.io/V2V_Web/>**

---

## Highlights

### 🟢 V2V-Zero — A Training-Free Framework

V2V-Zero exposes a latent visual-to-visual route already present in VLM-conditioned generators.
No weight updates, no learned modules, no adapters — only an inference-time conditioning wrapper.

```
I = G ( E ( V ) )
```

where `E` is a frozen multimodal VLM and `G` is a frozen diffusion generator. Replacing text `p` with a visual page `V` keeps the generator's learned interface and weights unchanged.

### 🟣 Simple-V2V Bench — A New Visual-Conditioning Benchmark

Seven visual-conditioning tasks evaluated under a unified protocol with a Qwen3-VL-32B judge:

| Tier | Tasks | Status |
| :--- | :--- | :--- |
| **Tier 1 — Strong** | Inline color, visual text | Already useful |
| **Tier 2 — Medium** | Inline visual ref, counting, style | Closed > open |
| **Tier 3 — Weak** | Pose, sketch | Open frontier |

Across **GPT Image 2, Seedream 5.0 Lite, Nano Banana 2, V2V-Zero, HunyuanVideo-1.5, Qwen-Image-Edit-2511, BAGEL-7B-MoT**, the benchmark exposes a consistent capability hierarchy: attribute binding is near-saturated, content generation is moderate, structural control remains hard for *all* evaluated systems.

### 📊 Main Numbers

| Benchmark | V2V-Zero | Best Baseline | Notes |
| :--- | :---: | :---: | :--- |
| GenEval (overall) | **0.85** | Qwen-Image 0.87 | Frozen weights, no fine-tuning |
| Simple-V2V Bench (overall) | **32.7** | GPT Image 2 · 64.7 | Top open-weight image score |
| Simple-V2V · Inline Color | **76.9** | GPT Image 2 · 92.4 | 2nd place overall |
| HunyuanVideo-1.5 (V2V) | **20.2** | — | Same interface, video extension |

---

## How V2V-Zero Works

V2V-Zero builds on a simple architectural observation: when a diffusion generator is trained to consume hidden states from a multimodal VLM, the frozen VLM **already** maps both text and images into the same conditioning space.

1. **Compose a visual specification page** — color swatches, rendered text, sketches, reference thumbnails, spatial diagrams.
2. **Encode via the frozen VLM** — extract final-layer hidden states `E(V)`.
3. **Inject into the frozen DiT** — cross-attend to `E(V)` through the existing conditioning slot.

Two conditioning modes are provided:

- **Image-HS-only** — non-reasoning visual control.
- **Full-Final** *(default)* — visual states + reasoning-token states; the DiT routes 95.0% of conditioning attention through visual states.

---

## Release Roadmap

We are preparing the public release. Stars and watches let us notify you when each component lands. ⭐

- [x] Project website with qualitative results · [https://yaofang-liu.github.io/V2V_Web/](https://yaofang-liu.github.io/V2V_Web/)
- [x] Paper on arXiv · [arXiv:2605.12271](https://arxiv.org/abs/2605.12271)
- [ ] **V2V-Zero inference code** — frozen Qwen-Image + Qwen2.5-VL + visual-page templates
- [ ] **Simple-V2V Bench** — 154 prompts × 7 categories, with rendering pipeline and Qwen3-VL-32B judge
- [ ] **Pretrained pages & checkpoints** — reproducible visual specification pages
- [ ] **HunyuanVideo-1.5 V2V extension** — video-side wrapper

---

## Citation

If V2V-Zero or Simple-V2V Bench helps your research, please cite:

```bibtex
@article{liu2026v2vzero,
  title   = {Beyond Text Prompts: Visual-to-Visual Generation as A Unified Paradigm},
  author  = {Liu, Yaofang and Cui, Kangning and Chu, Meng and Li, Zhaoqing and
             Zhang, Suiyun and Morel, Jean-Michel and Cun, Xiaodong and
             Che, Haoxuan and Liu, Rui and Chan, Raymond H.},
  journal = {arXiv preprint arXiv:2605.12271},
  year    = {2026}
}
```

---

## Authors

**Yaofang Liu**¹*, **Kangning Cui**², **Meng Chu**³, **Zhaoqing Li**⁴, **Suiyun Zhang**⁵, **Jean-Michel Morel**⁷, **Xiaodong Cun**⁶, **Haoxuan Che**⁵†‡, **Rui Liu**⁵*, **Raymond H. Chan**⁷

<sup>¹ City University of Hong Kong&nbsp;·&nbsp;² CityU (Dongguan)&nbsp;·&nbsp;³ HKUST&nbsp;·&nbsp;⁴ CUHK&nbsp;·&nbsp;⁵ Celia Research HK&nbsp;·&nbsp;⁶ Great Bay University&nbsp;·&nbsp;⁷ Lingnan University</sup>

<sup>‡ Project lead&nbsp;&nbsp;&nbsp;* Corresponding authors</sup>

---

## License

This repository is released under the [MIT License](LICENSE).
Datasets and model checkpoints will be released under their respective upstream licenses.

---

<div align="center">

**[🌐 Project Page](https://yaofang-liu.github.io/V2V_Web/)** &nbsp;·&nbsp;
**[📄 arXiv](https://arxiv.org/abs/2605.12271)** &nbsp;·&nbsp;
**[💻 Code (soon)](#release-roadmap)** &nbsp;·&nbsp;
**[🎯 Benchmark (soon)](#release-roadmap)**

<sub>Visual is a first-class conditioning language.</sub>

</div>
