---
title: "VRAM Explained: Why 128GB of RAM can't save a CUDA Crash"
date: 2026-10-18T20:29:43.972Z
author: Sitesh Pattanaik
---
### 1. Introduction

If you have run an AI model or worked on model training, you might have seen this error

> CUDA out of memory

You might have 64 or 128 GB of RAM and still hit it. The reason is VRAM.

Despite the name, VRAM (Video RAM), it has nothing to do with video in machine learning. It's just the memory lives on the GPU (Graphics Processing Unit). The name is something that stuck from when GPU was only used for processing graphical tasks.

### 2. Background

* A CPU has a few powerful cores. It's great at doing one complicated thing after another
* A GPU has thousands of small cores. It's great at doing the same simple math on huge piles of numbers at once.

As ML is highly dominated by Matrix multiplication which is nothing but simple math repeated billions of times, GPU dominates the world of AI. This is the whole reason VRAM exists.

### 3. VRAM vs System RAM

#### System RAM

* Lives on the Motherboard
* Managed by CPU
* Optimized for low latency (small, quick tasks)
* Yes, it's upgradable.

#### VRAM

* Lives on GPU card.
* Managed by GPU
* Optimized for High bandwidth and parallel processing tasks
* It's can't be upgraded, it's soldered on the chip.

*Example: a 7-billion parameter model stored in 16-bit (2 byte) needs about 7B * 2 bytes = 14GB, just for it's weights. That's before anything in the working memory.  So, the model size and VRAM is so tightly coupled.*

Now, that explain the original question that we had, why even having a 128GB of RAM isn't sufficient enough for CUDA.

### 4. What is a `Bus`?

A memory bus is simply the set of connections between two components. The length depends on the functionality.
This bus control, how fast data moves

* How wide the bus is
* How fast it's clocked

### 5. Two flavors of VRAM

#### GDDR6: The consumer standard.

* Memory chips sit flat on the circuit board, around the GPU.
* Narrow bus but very high frequency
* Found in gaming GPU and entry-level ML workstations.

#### HBM3e: The AI data center standard

* HBM == High Bandwidth Memory.
* Memory chips are stacked vertically like skyscraper.
* The floors are connected by Through-Silicon Vias (TSVs): microscopic copper-filled holes drilled through the silicon.
* The stacks sit beside the GPU on a `silicon interposer`
* Each stack has roughly 1024-bit bus. With 6-8 stacks, that's roughly 6000 thousands.

### 6. Conclusion: Memory is the bottleneck

Modern AI chips can compute much faster than memory can feed them. This is often called as ***memory wall***. Much of today's AI hardware innovation is moving the memory close to the chip, and tearing down the wall.

VRAM size and memory bandwidth matters a lot in the world of machine learning beside just raw compute power.
