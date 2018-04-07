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
In traditional parameterized CNN, a single conv layer transforms a 3D tensor of shape (`C_in, H_in, W_in`) to 3D tensor of shape (`C_out, H_out, W_out`) by computing cross-correlation of 3D volume at every `x` and `y` dimension of the input tensor with `C_out` seperate filters. (`C`: no. of channels of tensor; `H`: height of the tensor; `W`: width of the tensor). For each `C_out`, learned parameters involved in this transformation consists of a set of real no. that constitute the weights of the filter and a bias term.

In spectral filter param....
- Learned parameters are the complex coefficients of the DFT of the given filter (vs filter weights in traditional approach).
- Given an input tensor and complex numbers that constitute the spectral parameters for the filter, the output tensor is computed by first computing the inverse DFT of the filter and then convolving the resulting filter with the input tensor as usual. 
- Variables corresponds to complex DFT coefficients instetad of filter weights (TensorFlow implementation).
See Image: []()

2. Spectral Pooling:
In traditional CNNs, operation of network gradually transforms an input image with 3 channels: RGB and a large height and weight dimension to an output "image" consisting of a large no. of channels and a small no. of "pixels". They use max-pooling to downsample the image periodically; for example cut `H` and `W` by roughly half using max-pooling. 

In spectral pooling, dimensionality reduction is instead performed by:
- Computing the 2D DFT of input tensor for each input channel.
- Truncating the resulting freq matrx to the desired output dimensionality.
- Computing the inv DFT of the truncated freq matrix to obtain the output tensor for the given channel.

Spectral Pooling flowchart:
- Input Image 
- Take DFT 
- Crop Image from corners
- Treat Corner Cases?? 
- Apply Freq Dropout
- Take IFT
- Normalize image to min 0 and max 1
- Output Image
*Note: the DFT of a real-valued signal in the spatial domain obeys certain complex symmetries in the freq domain, specifiically: `F[s, t]` is the complex conjugate of `F[-s, -t]` and for a finite freq matrix, if `(s, t)` and `(-s, t)` coincide modulo the dimension of the matrix, then `F[s, t]` must be real-valued. Eg. DC components `F[0,0]` must be real valued and for (let's say) 16x16 freq matrix, `F[8,8]`, `F[0,8]` and `F[8,0]` must also be real valued.*
**As a result, when an even-numbered output dimension is desired, the input frequency matrix cannot simply be truncated; the complex symmetries governing the original frequency matrix are not the same as the ones governing the truncated matrix. The paper refers to the need to handle these corner cases carefully but does not include the algorithm itself.**

Pseudo Code: 
- Input: 2D `NxN` Fourier transform matrix of complex coeff. where the DC compoonent is located in the `[0,0]` top-left corner of the matrx.
- Output: 2D `MxM` (M<N) FFT matrix of complex coeff. where the DC comp is located in the `[0,0]` top-left corner of the matrx.
- If `M` is odd, 
  - KeepDim = 

### Caveats
