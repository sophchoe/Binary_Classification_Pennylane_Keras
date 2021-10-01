# Fraud Detection
Binary classification of genuine vs fraudulent for credit card transactions using classical-quantum hybrid model. 

## Code
This code is a Keras-Pennylane implementation of the "Supervised quantum neural networks" proposed in "Continuous-variable quantum neural networks". https://arxiv.org/pdf/1806.06871v1.pdf The original implementation using Strawberry Fields and Tensorflow is found here. https://github.com/XanaduAI/quantum-neural-networks/blob/master/fraud_detection/fraud_detection.py Thanks to Pennylane Tensorflow plug-in, we can easily convert quantum circuits into Keras layers and use Keras's built in loss and optimizer functions.

The data is a record of 284,806 genuine and fraudulent credit card transactions. Each transaction is respresented by 29 features. The modifications to the original data are:
 - Fraudulent transactions are only 492. I resample the data to have the genuine vs fraudulent ratio to be 3:1, 1,476:492. 
 - The features are ordered based on Principal Component Analysis (in the order of importance), hece the paper suggests selecting only 10.
 - The resulting csv file in the folder contains 1,968 samples with 10 features and label.

As per 
The proposed classical-quantum hybrid model had two hidden layers, outputs a vector of length 14, encodes it in quantum state using various quantum gates, performs quantum computation using quantum layers. 

<img width="500" alt="fraud)model" src="https://user-images.githubusercontent.com/22792633/135196338-48f08a90-64c0-47f4-b72c-e460f7c7c06f.png">

I am using the mean square error, which is not true to the cost function of the paper. It is due to the fact Pennylane does not support state vector extraction yet.

The data flow of the code is
- Prepare data using Pandas, Scikit-learn, and convert to Tensorflow tensors. One-hot encoding is used for labels.
- Define classical layers 
  - 2 hidden layers with 10 neurons
  - output layer with 14 neurons
- Encode the output of the classical layers in quantum state, using
  - Squeezing gate
  - Interferometer (composed of beam splitter and rotation gates)
  - Displacement gate
  - Kerr gate
- Define quantum layers (initialize the parameters to be optimized)
- Create a Pennylane quantum circuit. The measurement returns a 2-dimensional vector to match the one-hot encoded labels.
- Create a hybrid Keras model (Pennylane plug-in converts the quantum layers as a Keras layer)
- Train using Keras loss function and optimizer

The paper suggests cutoff dimension 10. That would be a better approximation of the overall resulting quantum state. However the cutoff dimension of only 2 is giving a good result as well. Experimenting with different cutoff dimension is encouraged.
