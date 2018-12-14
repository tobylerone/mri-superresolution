# MRI Super-Resolution

This project implements two GAN architectures for performing super resolution of MRI data in dicom format. These are modeled after the design in the repo [Github/Tensor Layer/SRGAN](https://github.com/tensorlayer/srgan.git), which is an implementation of the paper, [Photo-Realistic Single Image Super-Resolution Using a Generative Adversarial Network](https://arxiv.org/abs/1609.04802). We process low-resolution and high-resolution versions of MRI dicom images through the SRGAN (Super-Resolution GAN) architecture to perform super-resolution with the goal to speed up MRI by taking quicker, lower resolution scans of the patient.

The MRI data includes a sequence of low resolution slices and a sequence of high resolution slices for each patient in grayscale. The sequences of high resolution slices contain thinner slices, producing a longer sequence. 

The first model, --, performs a greedy matching algorithm to perform a one-to-one matching of low resolution to high resolution slices by selecting a nearby high resolution slice with the smallest distance, using position coordinate information embedded in the dicom file. Thus, a slice of size `(x-dim, y-dim, 1)` is inputted to the network to produce a high resolution slice of size `(x-dim, y-dim, 1)`.

The second model, --, improves upon --, performing the same greedy matching, but each high resolution slice is also paired with one slice before it and one slice after it in the sequence of slices. Thus, a slice of size `(x-dim, y-dim, 1)` is inputted to the network to produce a high resolution slice of size `(x-dim, y-dim, 3)`. This model attempts to learn features from the sequential nature of the slices.

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See running the project for notes on how to deploy the project on a live system.

### Prerequisites

vgg19.npz must be downloaded from [VGG](https://github.com/machrisaa/tensorflow-vgg), used for its pre-trained weights as part of a loss function. Additionally, tensorflow and tensorlayer must be installed.

### Files Information

Relevant files for the first model, --, are:
* `main.py`
* `config.py`
* `utils.py`
* `preprocess.py`
* `model.py`

Relevant files for the second model, --, are:
* `main2.py`
* `config2.py`
* `utils.py`
* `preprocess2.py`
* `model2.py`

Model configurations are located in `config[].py`. This includes
* Directory location containing data
* Model hyperparameters
Helper functions, such as for producing plots, are located in `utils[].py`. 
The GAN architectures, implemented with tensorflow and tensorlayer, are located in `model[].py`.
Preprocessing for the data to perform the matching and extract relevant information is done in `preprocess[].py`.
`main[].py` performs training of the models.

### Outputs

Checkpoints for the GAN are saved in `.npz` files, as are the losses calculated, placed in a new directory `models_checkpoints`. Plots are also generated for the losses.

## Running the project

To perform training of the model, execute 
```
python main.py
``` 
To perform evaluation, execute
```
python main.py --mode=evaluate
```
Images resulting from the evaluation are placed in a new directory `results`.

## References
* Ledig, Christian et al. “Photo-Realistic Single Image Super-Resolution Using a Generative Adversarial Network.” *2017 IEEE Conference on Computer Vision and Pattern Recognition (CVPR)* (2017): 105-114.

## Authors

* **Andrew Narcomey**, Stanford University, Department of Computer Science
* **Angela Gu**, Stanford University, Department of Mathematical and Computational Science