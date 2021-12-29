# Audio synthesis with Generative Adversarial Network

# Introduction

Audio synthesizing is important to the development of computer music, electronic music and even AI music generation. Nowadays, synthesizing audio for specific domains has many practical applications in creative sound design for music and film. The demand for audio is high for different purposes, such as musicians finding sound effects for specific scenarios.

One popular approach of audio synthesizing is using Generative adversarial networks (GANs). In current stage, GANs could enable rapid and straightforward sampling of large amounts of audio. Different models such as WaveGan, SpecGan, ParallenGan, GanSynth have their attempts on audio synthesis. They indeed produce some promising result and able to generate a lot of audio using small amount of time. However, the quality of result varies among different models and some of them has a lower matching human perception. Also, since audio is, unlike simple 2D images, time-domain data, the different representations of audio input will cause different restrictions during GANs training. Therefore, although GANs have seen wide success at generating images that are both locally and globally coherent, they have seen little application to audio generation.

# GAN models
In order to find out a better GAN model for audio synthesis, we involved three different kinds of GAN models to train. They are DCGAN, WGAN and WGAN-GP. We first test for the waveform approach to these three models, and then test for the spectrogram approach on the best model among these three GANs. The details of these models will be discussed in the later parts.

## DCGAN (proposed from the DCGAN paper)
Discriminator:

![alt text](https://github.com/div1121/ESTR3108-AI-Project/blob/main/Training%20Result/DCGAN/DCGAN-Discriminator-structure.JPG)

Generator:

![alt text](https://github.com/div1121/ESTR3108-AI-Project/blob/main/Training%20Result/DCGAN/DCGAN-Generator-structure.JPG)

## WGAN (proposed from the WGAN paper)
Similar structure as DCGAN.

In the training process, we update the discriminator model five more times than the generator each iteration. After each mini batch update in discriminator, the weights of discriminator model are constrained within [-0.01,0.01] in order to fulfill the Lipschitz constraint, which is weight clipping approach. Wasserstein loss function replaces the objective function in previous DCGAN to train the critic and generator models that promote larger difference between scores for real and generated images, where in implementation, the class label for real audio is 1 while for fake audio is –1.

## WGAN-GP (proposed from the WGAN-GP paper)
Similar structure as DCGAN.

The main difference from previous WGAN model is the replacement of gradient penalty instead of weight clipping. The additional term from above is added into the error function, to penalize the model if the gradient norm moves away from its target norm value 1, where the gradient is taken from the random sample point between the real audio and fake audio, which can ensure the Lipschitz constraint. We set the gradient penalty coefficient λ as 10 in our model.

## WGAN-GP-SPEC
Instead of taking waveform as input in WGAN-GP, the model takes spectrogram as input and follow the similar model in WGAN-GP.

Discriminator:

![alt text](https://github.com/div1121/ESTR3108-AI-Project/blob/main/Training%20Result/WGANGP-SPEC/WGANGP-SPEC-Discriminator-structure.JPG)

Generator:

![alt text](https://github.com/div1121/ESTR3108-AI-Project/blob/main/Training%20Result/WGANGP-SPEC/WGANGP-SPEC-generator-structure.JPG)

# Result

The following table (Table 1) shows the quantitative results for our different GAN models experiments with Nsynth dataset comparing real and generated data. A higher inception score suggests that semantic modes of the real data distribution have been captured. |D|_self indicates the intra-dataset diversity relative to that of the real test data. |D|_train indicates the distance between the dataset and the training set relative to that of the test data; a low value indicates a generative model that is overfit to the training data.

| Experiment    | Inception score | D_self | D_train|
| ------------- | --------------- | ------ | ------ |
| Real (train)  | 8.42±0.19       | 26.17  |    /   |
| Real (test)   | 7.50±0.51       | 28.84  | 30.92  |
| DCGAN         | 1.45±0.046      | 2.26   | 21.96  |
| WGAN          | 2.81±0.09       | 6.73   | 25.38  |
| WGAN-GP       | 7.12±0.15       | 22.92  | 23.99  |
| WGAN-GP-SPEC  | 3.53±0.08       | 3.97   | 4.46   |


