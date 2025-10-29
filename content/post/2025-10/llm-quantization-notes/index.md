---
title : 'LLM Quantization Notes'
date : 2025-10-29T12:08:35+03:00
format: hugo-md
categories:
  - basics
tags:
  - LLM
  - Quantization
---

These are my notes, with information taken from pdawg’s awesome blog post; I will continue adding more quantization-related insights to this page.

# Overview

- Quantization is a key technique for reducing the size and speeding up inference of Large Language Models (LLMs) by representing their weights and activations with fewer bits—without heavily degrading accuracy.

- Problem: Modern LLMs require large gigabytes of VRAM for full-precision inference—often exceeding even high-end GPUs’ memory. Solution: Quantization reduces each parameter’s bit-width (e.g., from 16 or 32 bits down to 8, 4, or fewer)

- Bit/Byte: 1 byte = 8 bits. More bits → more precision and range.
Floating-Point Numbers: Use sign bit, exponent, and mantissa to represent large and small values.
float32: 32 bits (full precision)
float16 / bfloat16: 16 bits, common in LLM training

- **Challenges**: Naive quantization (e.g., simple INT8) often fails for LLMs because of:
  - Outlier activations (extreme values that stretch the quantization range)
  - Error accumulation across layers

# Quantization Techniques

A. Quantization Aware training (QAT)
- Train or fine-tune a model with the lower-precision operations 
- QAT : Lower precision and finetune.
- EfficientQAT : Done in two steps 
  - Block-AP (Block-wise Training of All Parameters): Quantize one block at a time and jointly train weights + quantization parameters.
  - E2E-QP (End-to-End Quantization Parameter Training): Freeze weights and fine-tune only quantization parameters across blocks.

B. Post Training Quantization(PTQ)
- Convert model weights trained in high precision to a lower precision. Cheap and Fast. 
  - A trained FP32/FP16 model + Choose target bit-width (e.g., INT8)  →  Run calibration on representative data  → Quantize weights → Save quatized model

- But for sure there will be some errors for rounding. 
  - SmoothQuant mitigates activation outliers by shifting part of the scaling from activations to weights, keeping activations within a tighter range for cleaner INT8 quantization without altering the model’s output.
  - GPTQ preserves model accuracy by quantizing weights sequentially, prioritizing those that least affect the output to minimize overall error.
  - ZeroQuant enables scalable LLM quantization by applying group-wise weight compression

# Resources
- https://medium.com/@prathamgrover777/intro-to-quantization-in-llms-adfae03bf447
