# How Diffusion Models Work

This repository is inspired by the course "How Diffusion Models Work", taught by Sharon Zhou, CEO of Lamini. The course provides a comprehensive understanding of the technical details behind diffusion models, which power Midjourney, DALL·E, and Stable Diffusion. In addition, the course includes practical examples and code to generate video game sprites in a Jupyter notebook: https://lnkd.in/gBqUKuwV.

___

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

___

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

___

## Lab 3: Context

The code enables the generation of images with specific contexts such as side-facing characters, human characters, food, spells, and more.

#### Example 1: Generate images with predefined context

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


#### Example 2: Generate images with a mixed context

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

___

## Lab 4: Fast Sampling

### DDIM (Denoising Diffusion Implicit Model)
The sample_ddim function performs fast sampling using the DDIM algorithm. It takes the number of samples to generate and the number of intermediate steps as inputs. It initializes the samples with random noise and iteratively denoises the samples by predicting the noise at each time step using the trained model. The function returns the final generated samples and intermediate samples for visualization.

### DDPM (Denosing Diffusion Probabilistic Model)
The sample_ddpm function performs fast sampling using the DDPM algorithm. It follows a similar process as the DDIM algorithm but adds random noise back to the denoised samples to avoid collapse. The function returns the final generated samples and intermediate samples for visualization.

### Comparing DDPM and DDIM Speed
The denoise_add_noise function is a helper function used in both sampling methods. It removes the predicted noise from the samples and adds some random noise back to avoid collapse.

To compare the speed of DDPM and DDIM, the %timeit magic command is used to measure the execution time of each sampling function. The sample_ddim function is called with 32 samples and 25 intermediate steps, while the sample_ddpm function is called with 32 samples. The execution time for each function is displayed as the output.

### Usage
To use this code, follow these steps:
1. Install the required dependencies mentioned in the setup section.
2. Load the trained model weights using the `nn_model.load_state_dict` function. Adjust the file path to match the location of the model weights.
3. Call the sample_ddim or sample_ddpm function with the desired parameters to perform fast sampling.
4. Visualize the generated samples using the plot_sample function or any other preferred method.

### Note
The code assumes that the trained model weights are available in the specified save_dir. If the weights are not available, the code will throw an error. Make sure to train the model and save the weights before using this code.
