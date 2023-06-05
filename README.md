# How Diffusion Models Work

This repository is inspired by the course "How Diffusion Models Work", taught by Sharon Zhou, CEO of Lamini. The course provides a comprehensive understanding of the technical details behind diffusion models, which power Midjourney, DALL·E, and Stable Diffusion. In addition, the course includes practical examples and code to generate video game sprites in a Jupyter notebook: https://lnkd.in/gBqUKuwV.

## [Lab 1] Image Denoising with Stable Diffusion

The core of the algorithm is based on a deep learning model called ContextUnet, which is capable of predicting and subtracting noise from images to achieve high-quality denoising results.

### Key Features:

- ContextUnet Model: The repository provides the implementation of the ContextUnet model, which incorporates a U-Net-like architecture with residual blocks and group normalization. The model takes an input image, time step, and context label to predict the denoised image.
- Sampling Process: The repository includes a sampling process based on the Diffusion-Driven Probabilistic Models (DDPM) algorithm. It iteratively denoises the images by progressively removing noise, while incorporating additional noise to maintain stability during the denoising process.
- Pre-Trained Weights: Pre-trained weights for the ContextUnet model are provided, allowing users to immediately start using the model for image denoising tasks.
- Visualisation: The repository includes functions to visualize the denoising process by generating animations of the intermediate denoised images.

### Usage:

1. Clone the repository and install the required dependencies.
2. Load the pre-trained weights for the ContextUnet model.
3. Utilise the sampling process to denoise images, specifying the desired number of samples and save rate.
4. Visualise the denoising results through generated animations.
