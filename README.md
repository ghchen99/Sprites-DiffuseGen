# How Diffusion Models Work

This repository is inspired by the course "How Diffusion Models Work", taught by Sharon Zhou, CEO of Lamini. The course provides a comprehensive understanding of the technical details behind diffusion models, which power Midjourney, DALL·E, and Stable Diffusion. In addition, the course includes practical examples and code to generate video game sprites in a Jupyter notebook: https://lnkd.in/gBqUKuwV.

## Lab 1: Image Denoising with Stable Diffusion

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

## Lab 2: Denoising Diffusion Probabilistic Models (DDPM)
This repository contains an implementation of the Denoising Diffusion Probabilistic Models (DDPM) algorithm for image denoising. The code includes the training step for a ContextUnet model, which is trained to predict and remove noise from input images.

### Training Process
- Model Architecture: The ContextUnet model is utilized, consisting of an encoder-decoder architecture. It takes an input image, time step, and optional context label. The model employs convolutional layers, downsampling, residual blocks, and skip connections to extract features and reconstruct the denoised image.
- Hyperparameters: Various hyperparameters are set, such as the number of timesteps, beta values, network dimensions, and training parameters.
- Data Preparation: A custom dataset is loaded, containing image samples and corresponding labels. The DataLoader is used to handle batch loading and shuffling of the data during training.
- Optimisation: An Adam optimiser is initialised to update the ContextUnet model's parameters during training. The learning rate and other optimizer settings are specified.
- Noise Schedule: A noise schedule is constructed based on the beta values, defining how the noise level changes over time during the denoising process.
- Training Loop: The training loop iterates over the specified number of epochs. Within each epoch, the dataset is iterated over in batches. The model predicts the noise for each image at the current timestep and updates its parameters using backpropagation and the optimizer.
- Saving Intermediate Results: Intermediate results are periodically saved during training for visualization purposes. The denoised samples are generated by progressively removing noise while maintaining stability by adding some noise back in. These samples can be used to monitor the training progress and evaluate the denoising performance of the model at different epochs.

## Lab 3: Context

The code enables the generation of images with specific contexts such as side-facing characters, human characters, food, spells, and more.

### Example 1: Generate images with predefined context

```python
import torch
from diffusion_utilities import show_images, sample_ddpm_context

# Define the context tensor
ctx = torch.tensor([
    # hero, non-hero, food, spell, side-facing
    [1, 0, 0, 0, 0],
    [1, 0, 0, 0, 0],
    [0, 0, 0, 0, 1],
    [0, 0, 0, 0, 1],
    [0, 1, 0, 0, 0],
    [0, 1, 0, 0, 0],
    [0, 0, 1, 0, 0],
    [0, 0, 1, 0, 0],
]).float().to(device)

# Generate images with the specified context
samples, _ = sample_ddpm_context(ctx.shape[0], ctx)

# Display the generated images
show_images(samples)
```


### Example 2: Generate images with a mixed context

```python
import torch
from diffusion_utilities import show_images, sample_ddpm_context

# Define the context tensor
ctx = torch.tensor([
    # hero, non-hero, food, spell, side-facing
    [1, 0, 0, 0, 0],      # human
    [1, 0, 0.6, 0, 0],
    [0, 0, 0.6, 0.4, 0],
    [1, 0, 0, 0, 1],
    [1, 1, 0, 0, 0],
    [1, 0, 0, 1, 0]
]).float().to(device)

# Generate images with the specified context
samples, _ = sample_ddpm_context(ctx.shape[0], ctx)

# Display the generated images
show_images(samples)
```
