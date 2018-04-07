# Spectral Representations for Convolutional Neural Networks


1. Using Spectral Filter Parameterization as an alternate formulation for the learned weight matriices of CNNs.
2. The parameters are represented as the complex coefficients of the DFT of the traditional convolution filters.

3. Novelty in the paper:
- The authors "claim" that this formulation is more suitable for optimization than the traditional approach (Authors compare the convergence time of CNNs parameterized with spatial to CNNs parameterized with Spectral filters.)
- Authors also prose SPECTRAL POOLING as an alternative to MAX POOLING for dimensionality reduction in the image as it is transformed throughout the neural network. 
- Spectral pooling preserves information better than max-pooling and they quantify by comparing the approx loss between two techniques.
- They propose a network architecture consisting of alternating conv. and spectral pooling layers in which the only form of regularization is freq dropout and weight decay. 
- Authors achieve impressive accuracy rates on CIFAR-10 and CIFAR-100 datasets by optimizing the hyperparameters of their architecture.

4. Results: 
- Quantitively compared information preserving results between spectral pooling and max pooling, measured as the L2 norm of the difference of the pooled and unpooled images.
- Error rates of 8.6% and 31.6% on CIFAR-10 and CIFAR-100 respectively. 
- Improved convergence time vs standard CNN (Using Adam Optimizer). They measure this by comparing the no. of training epochs for traditionally parameterized CNN vs spectrally parameterized CNN to converge to the same error rate.

### Implementation:
1. Spectral Filter Parameterization:
- Learned parameters are the complex coefficients of the DFT of the given filter (vs filter weights in traditional approach).
- Given an input tensor and complex numbers that constitute the spectral parameters for the filter, the output tensor is computed by first computing the inverse DFT of the filter and then convolving the resulting filter with the input tensor as usual. 
- Variables corresponds to complex DFT coefficients instetad of filter weights.
See Image: []()

2. Spectral Pooling:


### Caveats
