# Text-to-Image Synthesis using DCGAN

## Introduction

This project generates images from text descriptions using a Deep Convolutional Generative Adversarial Network (DCGAN). The training data is collected from 102 different species of flowers along with corresponding descriptions of each image.

Link to datasets: [Flower Dataset](https://drive.google.com/drive/folders/173rCnPRMOd4kcYDxc2saaRfl7kBVa6TF)

### Applications of Text-to-Image Synthesis:
- Create images for game models or virtual environments from user descriptions.
- Create illustrations for stories or articles.
- Support the creation of diverse data for machine learning datasets.

### Examples:
1. **Text:** "This small bird has a brown breast and crown, and black primaries and secondaries."
   - **Image:** ![Bird](https://github.com/user-attachments/assets/7a8e9ef7-087d-4b9b-9857-0c5ead6cfb62)

2. **Text:** "The flower has petals that are bright pinkish purple with yellow stigma."
   - **Image:** ![Flower](https://github.com/user-attachments/assets/5fc601b7-310e-492f-9eb3-ad0e75bb30ce)

In this article, I use DCGAN in combination with embedding of the input text to create the desired data.

## 1. Overview of GAN

## Generative Adversarial Network (GAN)

GAN (Generative Adversarial Network) consists of two main models: the Generator and the Discriminator. Below is an overview of how these models work:

1. **Noise Vector**: GAN starts with a noise vector as input. This vector is processed by the Generator to produce fake data.

2. **Generator**: The Generator’s goal is to create fake data that is indistinguishable from real data, thus deceiving the Discriminator. It achieves this by optimizing the following loss function:

**Generator Loss Function**:

L_G(z) = -log(D(G(z)))

Where:
- `z` is the input noise vector.
- `G(z)` is the fake data generated by the Generator.
- `D(G(z))` is the probability that the Discriminator classifies the fake data `G(z)` as real.

The Generator aims to minimize this loss function, making `D(G(z))` close to 1, meaning the Discriminator perceives the fake data as real.

3. **Discriminator**: The Discriminator’s objective is to accurately distinguish between real and fake data. It achieves this by optimizing the following loss function:

**Discriminator Loss Function**:

L_D(z, x) = -y * log(D(x)) - (1 - y) * log(1 - D(G(z)))

Where:
- `x` is the real data.
- `y` is the label for the real data (`y = 1` if the data is real, `y = 0` if the data is fake).
- `D(x)` is the probability that the Discriminator classifies the real data `x` as real.
- `D(G(z))` is the probability that the Discriminator classifies the fake data `G(z)` as real.

The Discriminator aims to minimize this loss function, making `D(x)` close to 1 for real data and `D(G(z))` close to 0 for fake data.

During training, the Generator and Discriminator engage in a competitive process: the Generator works to produce more convincing fake data, while the Discriminator works to improve its ability to distinguish between real and fake data. This adversarial process helps enhance the quality of the generated data and the accuracy of the Discriminator.

### GAN Architecture

![GAN Architecture](https://github.com/user-attachments/assets/041e1bf1-7cff-4a9a-8078-af8cf8c22b69)

### GAN Training – Update G

![Update G](https://github.com/user-attachments/assets/44206fa6-1c79-4db9-957b-153ade7e32b1)

### GAN Training – Update D

![Update D](https://github.com/user-attachments/assets/002edddb-b192-411f-a0bb-3b18511de57b)

## 2. Overview of DCGAN

![DCGAN Overview](https://github.com/user-attachments/assets/38a54c8e-4b6e-46d9-9032-d3682bb894f1)

### Generator

![Generator](https://github.com/user-attachments/assets/e888d2bc-1298-47b7-87c7-e8c5f141b12d)

The noise vector is converted into a tensor and passed through a deep convolutional network, resulting in a tensor corresponding to the input size of the image.

### Discriminator

![Discriminator](https://github.com/user-attachments/assets/8fec86a9-b628-428c-8e46-21a99bf07a44)

The Discriminator receives images from two datasets: fake images generated by the Generator and real images from the dataset. These images are passed through a deep convolutional network, and the output is transformed into real/fake labels.

## 3. Text-to-Image Synthesis

![Text to Image](https://github.com/user-attachments/assets/e52a13aa-4487-47e2-bc4b-5b3429caafa8)
![Text to Image](https://github.com/user-attachments/assets/07bc5394-3653-4202-b41e-293dfa9027cf)

Connect the Text Described to Noise Vector

![Connect Text to Noise Vector](https://github.com/user-attachments/assets/7bf38888-0a7e-46e1-ba0c-d22e2310fa68)

The text description after that is converted from a vector to a tensor corresponding to the last layer in the Discriminator network. Then, an additional CNN layer is added to establish the relationship between the image and text before producing the output.

![Connect Text to Noise Vector](https://github.com/user-attachments/assets/b4bc7c72-793a-42e8-bcfe-57b584f7afd6)

### From Text Description to Image

### Model Enhancements Discriminator

![From Text Description to Image](https://github.com/user-attachments/assets/c14bd60f-b4e1-4f39-90d5-b9133e6dbcfe)
![From Text Description to Image](https://github.com/user-attachments/assets/28cca903-3104-471c-b001-14478cd83954)

Add the wrong description for the discrimination network to increase the accurate output evaluation.

![From Text Description to Image](https://github.com/user-attachments/assets/ee624b14-c25f-4d45-8522-5e2cc5b01232)
![From Text Description to Image](https://github.com/user-attachments/assets/1bc92fd5-4d10-4a47-acff-8c1194a70ba1)

### Model Enhancements Generator

1. Add additional embedded text to wrongly describe the image into real images, online to discriminate to help improve the ability to distinguish the background of the model.

   ![Model Enhancements](https://github.com/user-attachments/assets/33a65f43-c518-4283-838c-3297868d4c17)

2. Add an L1 loss between the generated image and the real image as a measure of similarity between pixels, attempting to bring the generated image closer to reality. With α as the hyperparameter.

   ![L1 Loss](https://github.com/user-attachments/assets/d63c5718-eb80-4aa4-9b88-43b7b93a391e)

3. Add an L2 loss as a measure of similarity between the model's evaluation of generated and real data. With β as the hyperparameter. This allows the model to improve quality further.

   ![L2 Loss](https://github.com/user-attachments/assets/6b170f47-685d-4d9a-af7d-38ec2ca41510)

## 4. Summary

This project uses DCGAN combined with embedding of input text to create desired images from text descriptions.

![Summary](https://github.com/user-attachments/assets/bc2e8105-61e8-4418-b187-73b6214c3a73)

