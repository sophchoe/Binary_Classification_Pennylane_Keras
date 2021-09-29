# Binary_Classification_Pennylane_Keras
Classical-Quantum hybrid model for credit card fraud detection

This code is a Keras-Pennylane implementation of the "Supervised quantum neural networks" proposed in "Continuous-variable quantum neural networks". https://arxiv.org/pdf/1806.06871v1.pdf

The data is a record of genuine and fraudulent credit card activities consisting of    records. The fraudulent activities are only %. The paper creates synthetic fraudulent data, to have genuine vs fraudulent ratio to be 3:1. I selected genuine data of the same ratio, creating a dataset of 1,968 samples.

The proposed classical-quantum hybrid model had two hidden layers, outputs a vector of length 14, encodes it in quantum state using various quantum gates, performs quantum computation using quantum layers. 

<img width="500" alt="fraud)model" src="https://user-images.githubusercontent.com/22792633/135196338-48f08a90-64c0-47f4-b72c-e460f7c7c06f.png">

I made a slight modification to the cost function of the paper, since Pennylane does not support state vector extraction yet.

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

The paper suggests cutoff dimension 10. That would be a better approximation of the overall resulting quantum state. However cutoff dimension of only 2 is giving a good result as well. Experimenting with different cutoff dimension is encouraged.
