# Variational Sparse Coding

This repository contains codes and examples implementing the variational sparse coding model for sparse variational inference. To run the code, you will need Python 3 and Tensorflow. If you use this model or these codes, please cite our paper:

@inproceedings{tonolini2019vsc,

  author    = {Francesco Tonolini and Bj\{o}rn Sand Jensen and Roderick Murray-Smith},

  title     = {Variational Sparse Coding},

  booktitle = {Proceedings of the Thirty-Fifth Conference on Uncertainty in Artificial Intelligence (UAI)},

  year      = {2019}

}

## Code Instructions

The main code to implement the model is "VSC_model.py", in the folder "models". This code implements three functions:


### 1) CF = VSC_model.train(training_data, parameters, save_directory)

This function trains the weights of the VSC model on the data set "training_data", which is accepted as a 2D array, where samples are along dimension 0 and elements per sample along dimension 1. The trained weights are saved as a Tensorflow .ckpt file in the directory "save_directory". The model takes a number of parameters stored in a dictionary "parameters" which has to contain the following values:

num_iterations - Number of totl iterations

initial_training_rate - Initial training rate for ADAM optimiser

batch_size - Batch size

report_interval - Interval of iterations at which to display and save values (ELBO, KL ecc.)

z_dimension - Latent space dimensionality

h_enc_dim - Number of hidden units between observation and latent space 

h_dec_dim - Number of hidden units between latent and observation space

n_pinputs - Number of pseudo-inputs constituting the prior

warm_up_start - Number of iteration below which only discrete variables can be used (Spike warmup)

warm_up_end - Number of iteration above which both discrete and continuous variables can be used (Spike warmup)

shorten_steps_it - Iteration number after which to reduce the step size

nu - Inverse "temperature" of sigmoid to be used for parametrising discrete variables (the higher the sharper)

alpha - Overall sparsity to be encouraged in the aggregate posterior

The function also outputs several vectors containing function values at different iterations contained in the dictionary "CF". These are:

cost - The value of the VSC cost function

KL_divergence - The average KL divergence between the recognition models and the pseudo-inputs

reconstruction_likelihood - The average reconstruction likelihood for the data in the batch

aggregate_KL = The divergence between the average Spike probability value of the pseudo-inputs and the imposed sparsity alpha


### 2) z, z_act = VSC_model.encode(test_data, parameters, load_directory)

Using the weights stored in "load_directory", this function encodes the samples in "test_set", provided in the same format as "training_set" above. The input parameters are provided in the same way described above through the dictionary "parameters". The function returns two outputs: the encodede sparse latent variables "z" and the corresponding Spike values "z_act" (i.e. the binary activation of latent features).


### 3) mu_x, sig_x = VSC_model.decode(z, np.shape(training_data), parameters, load_directory)

Using the weights stored in "load_directory", this function generates means "mu_x" and standard deviations "sig_x" from sparse latent variables "z". The parameters are given through the dictionary "parameters" as described above.


## Examples

We provide two examples where this code is implemented to train VSC and subsequently use it as a sparse encoder. The example "fashionMNIST_example.ipynb" trains the VSC model on examples from the fashion-MNIST dataset and visualises the average latent elements activation for each class in the data set. The example "Smiley_example.ipynb" trains the model on an artificial dataset generated with known source features in order to tesat disentanglement.

## Sparse Artificial Dataset

The sparse artificial dataset we use for our evaluation is included in the folder "Data_Sets/Smiley" along with the Matlab code used to generate it. This code is "make_smile_dataset.m" and can be found in "Data_Sets/Smiley/Generate_Smiley_Dataset". The code can be used to generate different datasets with varying sparsity for the attributes (the datasets used in the paper and the example here are generated with a sparsity coefficient of 0.5).
