# Variational_Sparse_Coding
This repository contains codes and examples implementing the variational sparse coding model for sparse variational inference.

The main code to implement the model is "VSC_model.py", in the folder "models". This code implements three functions:

1) CF = VSC_model.train(training_data,parameters,save_directory)

This function trains the weights of the VSC model on the data set \textbf{training_data}, which is accepted as a 2D array, where samples are along dimension 0 and elements per sample along dimension 1. The trained weights are saved as a Tensorflow .ckpt file in the directory "save_directory". The model takes a number of parameters stored in a dictionary "parameters" which has to contain the following values:

num_iterations - Number of totl iterations
initial_training_rate - Initial training rate for ADAM optimiser
batch_size=500 - Batch size
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
