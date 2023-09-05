
## All Topics : 

### 1. Supervised Learning 

- I explored about this because when i started my research i got to know about that supervised learning is the most beginner friendly among them. 
- while researching, i came to know about that supervised learning is process in which data provided to model contains hint or labels basically labelled data.
- The main task of supervised learning is to construct an estimator able to predict the label of an object given by the set of features. Then, the learning algorithm receives a set of features as inputs along with the correct outputs and it learns by comparing its actual output with corrected outputs to find errors. It then modifies the model accordingly. In supervised, we train the model on a set of labeled data (training data) and then use the model to predict labels on unlabeled data (testing data).
- After all this, i started learning about Supervised learning algorithms :
	- Decision Trees  -> The decision tree consists of nodes that form so called root tree, which means that it is a distributed tree with a basic node called root with no incoming edges. The node that has outgoing edges is called internal node or a test node. The rest of the nodes are called leaves.Each leaf is assigned to one class that represents the most appropriate target value.![[Pasted image 20230904012602.png]]
	- Linear Regression -> Linear Regression's role is to find relationships and dependencies between variables.![[Pasted image 20230904012624.png]]
	- Naive Bayes : Assumes an underlying probabilistic model and it allows capturing uncertainly about the model in a principled way by determining probabilities of the outcomes. It calculates explicit probabilities for hypothesis and it robust the noise in input data.![[Pasted image 20230904012854.png]]
	- Logistic Regression : works by extracting some set of weighted features from the input, taking logs and combining them linearly, which means that each feature is multiplied by a weight and then added up. ![[Pasted image 20230904013221.png]]


### 2. Artificial Neural Network
- I explored about neural network because from very childhood, i was curious about Neural network works or how our brain remembers or learns everything. even though i not interested in Biology.
- Neural network is a computing model, by a large number of nodes (or neurons) connected to each other. Each node represents a specific output function, called the activation function.
- The output of the network will vary depending on how the network is connected, the weight value, and the incentive function.
- In an artificial neural network, a neuron processing unit can represent different objects, such as features, letters, concepts, or some meaningful abstraction pattern
- The type of processing unit in the network is divided into three categories: input unit, output unit and hidden unit. The input unit accepts signals and data from the outside world [5]. The output unit realizes the output of the system processing result. The hidden unit is a unit that is located between the input and output units and cannot be observed outside the system
- Four Basic Characteristics
1. Non-linear -> universal feature of nature. The brain’s intelligence is a non-linear phenomenon. Artificial neurons in the activation or inhibition of two different states, this behaviour in the mathematical performance of a nonlinear relationship.
2. Non-limited -> A neural network usually consists of multiple neurons connected together. The overall behaviour of a system depends not only on the characteristics of a single neuron but also on the interaction and interconnection of the units.
3. Non-qualitative -> Neural networks not only process the information can have a variety of changes, but also in dealing with the information, nonlinear dynamical system itself is constantly changing.
4. Non-convexity -> this function has multiple extrema, so the system has more stable equilibrium states, which will lead to the diversity of system evolution.

- ## Convolutional Neural Networks (CNNs)
	- The only notable difference between CNNs and traditional ANNs is that CNNs are primarily used in the field of pattern recognition within images.
- Limitations of Traditional ANNs :
	- The MNIST database of handwritten digits are most suitable form for ANN because dimension of image is just 784 weights (28 x 28 x 1), which is manageable
	- But, if you consider a input image of dimension 64x64. the number of weights on just a single neuron of the first layer increases substantially to 12, 288.
- when i learned about these limitations, the first thought that came to my mind is just increase the number of neurons. But i found it out wrong because increasing number of neurons not only requires more computation power but also leads to overfitting due to which if we provide train data again to model it will give max accuracy but when we provide any data which is not present in training set it will give a very poor result.
- The neurons that layer within the CNN will only connect to a small region of the layer preceding it.
- One of the key differences is that the neurons that the layers within the CNN are comprised of neurons organised into three dimensions, the spatial dimensionality of the input (height and the width) and the depth. The depth does not refer to the total number of layers within the ANN, but the third dimension of a activation volume.
- for the example given earlier, the input ’volume’ will have a dimensionality of 64 × 64 × 3 (height, width and depth), leading to a final output layer comprised of a dimensionality of 1 × 1 × n (where n represents the possible number of classes) as we would have condensed the full input dimensionality into a smaller volume of class scores filed across the depth dimension.

### CNN Architecture
- CNNs are comprised of three types of layers. These are convolutional layers, pooling layers and fully-connected layers
![[Pasted image 20230904150650.png]]

1. input layer will hold the pixel values of the image.
2. The convolutional layer will determine the output of neurons of which are connected to local regions of the input through the calculation of the scalar product between their weights and the region connected to the input volume.
3. The pooling layer will then simply perform down sampling along the spatial dimensionality of the given input, further reducing the number of parameters within that activation.
4. The fully-connected layers will then perform the same duties found in standard ANNs and attempt to produce class scores from the activations, to be used for classification.

- Now explaining layers of CNN layer by layer
- Convolutional Layer : focus around the use of learnable kernels. it activates all filter across the dimensionality of the input to produce a 2D activation map. These activation looks like : ![[Pasted image 20230904170008.png]]
- 
- Convolutional layers are also able to significantly reduce the complexity of the model through the optimisation of its output. These are optimised through three hyperparameters, the depth, the stride and setting zero-padding.
- training ANNs on inputs such as images results in models of which are too big to train effectively. This comes down to the fullyconnected manner of standard ANN neurons, so to mitigate against this every neuron in a convolutional layer is only connected to small region of the input volume. The dimensionality of this region is commonly referred to as the receptive field size of the neuron.
- Now, Let's Talk about three hyperparameters 
	- depth -> Reducing this hyperparameter can significantly minimise the total number of neurons of the network, but it can also significantly reduce the pattern recognition capabilities of the model.
	- stride -> is a hyperparameter in which we set the depth around the spatial dimensionality of the input in order to place the receptive field. For example if we were to set a stride as 1, then we would have a heavily overlapped receptive field producing extremely large activations. Alternatively, setting the stride to a greater number will reduce the amount of overlapping and produce an output of lower spatial dimensions.
	- zero-padding -> is the simple process of padding the border of the input, and is an effective method to give further control as to the dimensionality of the output volumes.
- Parameter-sharing -> works on the assumption that if one region feature is useful to compute at a set spatial region, then it is likely to be useful in another region. If we constrain each individual activation map within the output volume to the same weights and bias, then we will see a massive reduction in the number of parameters being produced by the convolutional layer.



- Pooling Layer -> aim to gradually reduce the dimensionality of the representation, and thus further reduce the number of parameters and the computational complexity of the model. The pooling layer operates over each activation map in the input, and scales its dimensionality using the “MAX” function. In most CNNs, these come in the form of max-pooling layers with kernels of a dimensionality of 2 × 2 applied with a stride of 2 along the spatial dimensions of the input. This scales the activation map down to 25% of the original size - whilst maintaining the depth volume to its standard size.Due to the destructive nature of the pooling layer, there are only two generally observed methods of max-pooling. Usually, the stride and filters of the pooling layers are both set to 2 × 2, which will allow the layer to extend through the entirety of the spatial dimensionality of the input. Furthermore overlapping pooling may be utilised, where the stride is set to 2 with a kernel size set to 3. Due to the destructive nature of pooling, having a kernel size above 3 will usually greatly decrease the performance of the model.

- Fully - connected layer
	- The fully-connected layer contains neurons of which are directly connected to the neurons in the two adjacent layers, without being connected to any layers within them. This is analogous to way that neurons are arranged in traditional forms of ANN.

- Another common CNN architecture is to stack two convolutional layers before each pooling layer, as illustrated in Figure 5. This is strongly recommended as stacking multiple convolutional layers allows for more complex features of the input vector to be selected.
- It is also advised to split large convolutional layers up into many smaller sized convolutional layers. This is to reduce the amount of computational complexity within a given convolutional layer. For example, if you were to stack three convolutional layers on top of each other with a receptive field of 3×3. Each neuron of the first convolutional layer will have a 3×3 view of the input vector. A neuron on the second convolutional layer will then have a 5 × 5 view of the input vector. A neuron on the third convolutional layer will then have a 7 × 7 view of the input vector. As these stacks feature non-linearities which in turn allows us to express stronger features of the input with fewer parameters. However, it is important to understand that this does come with a distinct memory allocation problem - especially when making use of the backpropagation algorithm. The input layer should be recursively divisible by two. Common numbers include 32 × 32, 64 × 64, 96 × 96, 128 × 128 and 224 × 224. Whilst using small filters, set stride to one and make use of zero-padding as to ensure that the convolutional layers do not reconfigure any of the dimensionality of the input. The amount of zero-padding to be used should be calculated by taking one away from the receptive field size and dividing by two.activation CNNs are extremely powerful machine learning algorithms, however they can be horrendously resource-heavy. An example of this problem could be in filtering a large image (anything over 128 × 128 could be considered large), so if the input is 227 × 227 (as seen with ImageNet) and we’re filtering with 64 kernels each with a zero padding of then the result will be three activation vectors of size 227 × 227 × 64 - which calculates to roughly 10 million activations - or an enormous 70 megabytes of memory per image. In this case you have two options. Firstly, you can reduce the spatial dimensionality of the input images by 10 Keiron O’Shea et al. resizing the raw images to something a little less heavy. Alternatively, you can go against everything we stated earlier in this document and opt for larger filter sizes with a larger stride (2, as opposed to 1).



- RECURRENT NEURAL NETWORKS
- Recurrent Neural Networks (RNNs) are a type of neural network architecture which is mainly used to detect patterns in a sequence of data. Such data can be handwriting, genomes, text or numerical time series which are often produced in industry settings. 
- RNNs find applications in Language Modelling & Generating Text, Speech Recognition, Generating Image Descriptions or Video Tagging.
- While Feedforward Networks pass information through the network without cycles, the RNN has cycles and transmits information back into itself ![[Pasted image 20230904184753.png]]




































- Deep Learning

-  in the case of highly varying functions, learning algorithms entirely based on local generalization are severely impacted by the curse of dimensionality [2]. Deep architectures address this issue with the use of distributed representations and as such may constitute a tractable alternative
- Unfortunately, training deep architectures is a difficult task and classical methods that have proved effective when applied to shallow architectures are not as efficient when adapted to deep architectures. Adding layers does not necessarily lead to better solutions. For example, the more the number of layers in a neural network, the lesser the impact of the back-propagation on the first layers. The gradient descent then tends to get stuck in local minima or plateaus [3], which is why practitioners have often preferred to limit neural networks to one or two hidden layers.
- This issue has been solved by introducing an unsupervised layer-wise pretraining of deep architectures [3, 4]. More precisely, in a deep learning scheme each layer is treated separately and successively trained in a greedy manner: once the previous layers have been trained, a new layer is trained from the encoding of the input data by the previous layers. Then, a supervised fine-tuning stage of the whole network can be performed
- Deep Learning with RBMs
	-  Restricted BM - An RBM defines a joint probability on both the observed and unobserved variables which are referred to as visible and hidden units respectively. The distribution is then marginalized over the hidden units to give a distribution over the visible units only. The probability distribution is defined by an energy function E. ![[Pasted image 20230904191912.png]] The variable v is the input vector and the variable h corresponds to unobserved features [7] that can be thought of as hidden causes not available in the original dataset.![[Pasted image 20230904191948.png]]
	- The energy function above is crafted to make the conditional probabilities p(h|v) and p(v|h) easy to control. The computation is done using the usual neural network propagation rule (see Fig. 2) with: ![[Pasted image 20230904192134.png]]
	- where sigm(x) is the logistic activation function.
	- E can be appropriately modified to define the Gaussian-Bernoulli RBM by including a quadratic term on the visible units ![[Pasted image 20230904192248.png]]
	- where σi represents the variance of the input variable vi.
	- Using this energy function, the conditional probability p(h|v) is almost unchanged but p(v|h) becomes a multivariate Gaussian with mean ai + σi j wijhj and a diagonal covariance matrix:
	- ![[Pasted image 20230905123900.png]]
	- In a deep architecture using Gaussian-Bernoulli RBM, only the first layer is real-valued whereas all the others have binary units.

- Learning with RBMs and Contrastive Divergence
- In order to train RBMs as a probabilistic model, the natural criterion to maximize is the log-likelihood. This can be done with gradient ascent from a training set D likewise:
![[Pasted image 20230905124136.png]]
- In practice, the number of steps can be greatly reduced by starting the Markov chain with a sample from the training dataset and assuming that the model is not too far from the target distribution. This is the idea behind the Contrastive Divergence (CD) learning algorithm [11]. Although the maximized criterion is not the log-likelihood anymore, experimental results show that gradient updates almost always improve the likelihood of the model [11]. Moreover, the improvement to the likelihood tend to zero as the length of the chain increases [12], an argument which supports running the chain for a few steps only. Notice that the possibility to use only the sign of the CD update is explored in the present special session [13].


### 2.3 From stacked RBMs to deep belief networks

- In an RBM, the hidden variables are independent conditionally to the visible variables, but they are not statistically independent. Stacking RBMs aims at learning these dependencies with another RBM. The visible layer of each RBM of the stack is set to the hidden layer of the previous RBM (see Fig. 3). Following the deep learning scheme, the first RBM is trained from the input instances and other RBMs are trained sequentially after that. Stacking RBMs increases a bound on the log-likelihood [14], which supports the expectation to improve the performance of the model by adding layers.
- ![[Pasted image 20230905125502.png]]
- A stacked RBMs architecture is a deep generative model. Patterns generated from the top RBM can be propagated back to the input layer using only the conditional probabilities as in a belief network. This setup is referred to as a Deep Belief Network.


### 3. Other Models and Variations

#### 3.1 Stacked Auto-Associators
- Another module which can be stacked in order to train a deep neural network in a greedy layer-wise manner is the Auto-Associator (AA).
- An AA is a two-layers neural network. The first layer is the encoding layer and the second is the decoding layer. The number of neurons in the decoding layer is equal to the network’s input dimensionality. The goal of an AA is to compute a code y of an input instance x from which x can be recovered with high accuracy. This models a two-stage approximation to the identity function:
- ![[Pasted image 20230905130506.png]]
- fenc is encoding function and fdec is decoding function.
- An AA can be trained by applying standard back-propagation of error derivatives. Depending on the nature of the input data, the loss function can either be the squared error LSE for continuous values or the cross-entropy LCE for binary vectors:
- ![[Pasted image 20230905130730.png]]
- The AA training method approximates the CD method of the RBM. Another important fact is that an AA with a nonlinear fenc differs from a PCA as it is able to capture multimodal aspects of the input distribution. Another important fact is that an AA with a nonlinear fenc differs from a PCA as it is able to capture multimodal aspects of the input distribution. Similarly to the parametrization in an RBM, the decoder’s weight matrix Wdec can be set to the transpose of the encoder’s weight matrix, i.e. Wdec = W enc. In such a case, the AA is said to have tied weights. The advantage of this constraint is to avoid undesirable effects of the training process, such as encoding the identity function, i.e. fenc(x) = x. This useless result is possible when the encoding dimensionality is not smaller than the input dimensionality.
- An interesting variant of the AA is the Denoising Auto-Associator (DAA) [18]. A DAA is an AA trained to reconstruct noisy inputs. To achieve this goal, the instance fed to the network is not x but a corrupted version x˜. After training, if the network is able to compute a reconstruction xˆ of x with a small loss, then it is admitted that the network has learned to remove the noise in the data in addition to encode it in a different feature space.
- ![[Pasted image 20230905131400.png]]
- Finally, a Stacked Auto-Associator (SAA) [3, 19, 18, 20] is a deep neural network trained following the deep learning scheme: an unsupervised greedy layer-wise pre-training before a fine-tuning supervised stage, as explained in Sect. 2.3 (see also Fig. 1). Surprisingly, for d dimensional inputs and layers of size k d, a SAA rarely learns the identity function [3]. In addition, it is possible to use different regularization rules and the most successful results have been reported with adding a sparsity constraint on the encoding unit activations [20, 21, 22]. This leads to learning very different features (w.r.t RBM) in the intermediate layers and the network performs a trade-off between reconstruction loss and information content of the representation [21].



### 3.2 Deep Kernel Machines

- The Multilayer Kernel Machine (MKM) has been introduced as a way to learn highly nonlinear functions with the iterative application of weakly nonlinear kernel methods.
- The authors use the Kernel Principal Component Analysis (KPCA) [24] for the unsupervised greedy layer-wise pre-training stage of the deep learning scheme. From this method, the  + 1th layer learns a new representation of the output of the layer  by extracting the n principal components of the projection of the output of  in the feature space induced by the kernel.
- the authors propose to apply a supervised strategy devoted to selecting the best informative features among the ones extracted by the KPCA. It can be summarized as follows: 
1. rank the nl features according to their mutual information with the class labels; 
2. for different values of K and ml ∈ {1 ...nl}, compute the classification error rate of a K-NN classifier using only the ml most informative features on a validation set;
3. the value of ml with which the classifier has reached the lowest error rate determines the number of features to retain.

- However, the main drawback of using KPCA as the building block of an MKM lies in the fact that the feature selection process must be done separately and thus requires a time-expensive cross validation stage. To get rid of this issue when training an MKM it is proposed in this special session [25] to use a more efficient kernel method, the Kernel Partial Least Squares (KPLS).
- KPLS does not need cross validation to select the best informative features but embeds this process in the projection strategy [26]. The features are obtained iteratively, in a supervised manner. At each iteration j, KPLS selects the jth feature the most correlated with the class labels by solving an updated eigenproblem. The eigenvalue λj of the extracted feature indicates the discriminative importance of this feature. The number of features to extract, i.e. the number of iterations to be performed by KPLS, is determined by a simple thresholding of λj .

### 3.3 Deep Convolutional Networks

- They can be seen as biologically inspired architectures, imitating the processing of “simple” and “complex” cortical cells which respectively extract orientations information (similar to a Gabor filtering) and compositions of these orientations.
- The main idea of convolutional networks is to combine local computations and pooling.
- The main idea of convolutional networks is to combine local computations (convolution of the signal with weight sharing units) and pooling. 
- The convolutions are intended to give translation invariance to the system, as the weights depend only on spatial separation and not on spatial position. 
- The pooling allows to construct a more abstract set of features through nonlinear combination of the previous level features, taking into account the local topology of the input data. By alternating convolution layers and pooling layers, the network successively extracts and combines local features to construct a good representation of the input. 
- The connectivity of the convolutional networks, where each unit in a convolution or a pooling layer is only connected to a small subset of the preceding layer, allows to train networks with as much as 7 hidden layers. The supervised learning is easily achieved, through an error gradient backpropagation.



### Tensorflow

- It is developed by Google and also a successor of DistBelief. 
- It is designed for large-scale machine learning model implementation and deployment.
- A TensorFlow computation is described by a directed graph, which is composed of a set of nodes. The graph represents a dataflow computation, with extensions for allowing some kinds of nodes to maintain and update persistent state and for branching and looping control structures within the graph.
- Values that flow along normal edges in the graph (from outputs to inputs) are tensors, Special edges, called control dependencies, can also exist in the graph: no data flows along such edges, but they indicate that the source node for the control dependence must finish executing before the destination node for the control dependence starts executingSpecial edges, called control dependencies, can also exist in the graph: no data flows along such edges, but they indicate that the source node for the control dependence must finish executing before the destination node for the control dependence starts executing. 
- control dependencies can also be used to control the peak memory usage.

































- https://www.mathworks.com/discovery/deep-learning.html#:~:text=Deep%20learning%20is%20a%20machine,a%20pedestrian%20from%20a%20lamppost.
- https://pdf.sciencedirectassets.com/274150/1-s2.0-S0939388919X00021/1-s2.0-S093938891830120X/main.pdf?X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLWVhc3QtMSJHMEUCIQC2dJy7sEKECJ3fJDWqCGgUJWg9i6BOO8uBI9cpqRS3QwIgMYcKxEbDyuUAA8I9%2F5pDmpyrd4PVuoDS9L%2BoV7lMwB8qsgUIRRAFGgwwNTkwMDM1NDY4NjUiDMtpZB58jaQLJfMDXCqPBUVmu%2BzCCPsM2J4WKhHp9sgE6nE4GSur16Clvv8SwjnyLF8oynR7WZ%2FFLGlGS8qaji4jrO%2F5REZfoKEfHH7ODRqJvk%2BWo6WN9jaZQFBC%2BF9MFiP5c7qS4QygIZfRH6592OTBWj2P1tgpTMXAjqAaYed10hN0zYygAYRJO%2BZjROBuKvARSkqUPmfVriNlo4GR22qe8iwU3XfenMHI4AB0L3YCdFx7rj%2Ba84r4Ugfvxgzh9f2wbwVhKRWIVX2x%2BUUMDeVRG3zxaONed8TI%2BdP%2BmumLNcaQX%2FqL%2BGgz%2FBT2%2BvgMOKmTiYlX54Lu9Ru2YfoOkCNtlDUWmXQfNyv9iOaxPkRsXgQojASlM0%2Bo66zS80ODnL8dDLwDsjlXqs9xS41iuAN%2FEGYIn4ZGTJUW5v%2BqNk1vEzZ5Q0hf1o62LcC0Bk9KvisB4KhhcTfI7j0OKvjQFXAhy8omIcWM7wykl4Dr0TZtmXDdzhREnIT1KJQsvR0PPCc%2BWsOpyS%2BRsoZW1ZlmLt8wiNfrQgFM%2BxUtSqPM7p8uHGtO4bC0B%2FplO3uakekofw%2FJyVnlE0n4qUSHUxOCIiF5HjwV2kC4HlgpkC4gei%2BDfxvqSwfUtbWaAZjFYxX%2BgUauU7lDEOTVp3nO2MWKpAHnJsh%2BaSA15MxkzCrrOG98dvls9T0tDvY0%2FsZh5IilZSVT5vQBUbA5lFccqhgJhgtIIvolJd%2FcGsCtGXhgfYfmM2A4a0ZBx7dFEjN%2F7K305DmhIXa3IHtKostL8Wcu6UCdh51kEpwp2Jciec3cAcv7JQvjFgIXOKhr%2FvM%2B2wVndza4%2BxY49lb7LUp7uSEWHFpuqsgbF4sHFbr95o3Y97Bok2ZrFhXSYIogib3FfGYwkZLXpwY6sQG2UbiYxMymBRkleglOdN7ajT3DHuEsIh0MSjxhOSEPRlJlwDaMk%2F174zFF18anrdAGdVylyJEvvUI8eZ%2FGmSFiL74L72XpO7aJVFsBEsZwaQETermFhIC5h%2BT7YVVPssGQMcPGacxmxCV8WRXMVWWihnQnlNxIiOzMKYYS4Kb1ph%2BlWhNL3ybBTh9ISXzHKY4dBku1JgUAC0fxbHFpCmY6W0GxBm9cC80zndoRURxtAoY%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20230904T132219Z&X-Amz-SignedHeaders=host&X-Amz-Expires=300&X-Amz-Credential=ASIAQ3PHCVTY3O3AVWRF%2F20230904%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=b65409668485e3ef312f6300196f27900c6123557ae2e763e378b861e621c274&hash=353cc6cf53ab2aa1129a217c9aff6d00f1edbdc19db7d03ce8652a5d39d7f49e&host=68042c943591013ac2b2430a89b270f6af2c76d8dfd086a07176afe7c76c2c61&pii=S093938891830120X&tid=spdf-f38eabe0-3af3-490e-a5d1-2ba97eb49f68&sid=ffa363649cda874be43b0b855c8d221bcb32gxrqb&type=client&tsoh=d3d3LnNjaWVuY2VkaXJlY3QuY29t&ua=1308585355585f0f535006&rr=80168a67dfdf1bc8&cc=in
- https://pdf.sciencedirectassets.com/271125/1-s2.0-S0893608020X00074/1-s2.0-S0893608020302197/main.pdf?X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLWVhc3QtMSJHMEUCIBhhhnNyeQ9amnxKI9p9czD%2BjPpCGxBB%2BDYyV8wdMPRQAiEA6qw5xI0Nblyk%2B64yGHy1%2FtY6enhH0lRSp0yMkX1YXVAqswUIRRAFGgwwNTkwMDM1NDY4NjUiDJwRM21mv5J2qRjiYiqQBbdtCKf8NnWC6KzUxQrZoF2rDKJki4tyCMLoxgI%2BxxDvVsEPMlzFlu8J%2Bchq1NFJ2cPgQvSzeifqifQDsxjtJd%2BYnPrLdzMeiy%2FljlHx%2BGpAJkPyukF4MGiPyy7MQSnGmUXR46AFZSQvxA2Ge82voF8CsnA9qk8zPrQGkYUzGPzKIeQVZT9oBikF1mdAavJgQlxY1ociTt1lO4Q%2FIkxr6KfWDj2u%2BWEOYb7lTfiWjhhXyjhme4zZIxzCuwE6N4J4nl2bNDBESGDJYVotgbQsyFR4M%2F%2Fl1vkFKoTrpl7%2FiHtvKhOkqYBM9pu0E9YDVdgiFGvEhiSiFHnOS3bfJQZLc0ozJleCqOlpvPLXvZAZDQaznqi5gqQOwx5kbSgpRGX7B%2Bzpg05DRIrAZl5if20WZSeI7kAYDCuZYHZL1iwBanErSdbqsktUHzX2XH17a8vkRNdMBGXGpVWs%2FLr3OWrgqwXQYS4x5ULKPh66YLePJWs1nHJ%2BhacEXkU%2F2%2BgbY2d%2FYIKDcwO1zoHaDaObaVytjJpJMKw8IW6y2oFWiRgD5LtdrVR0VZ5wapDVKIa0LInRXtYmSxGmV3tKX7hR6wwfGQlRW7Zuc1lP%2B7v0JMLaNYmnmHiTtwZxq5V6q2QwkSvIZpC2hHBG4wwFPjLFUZ2iMES%2FLdod7%2FCTmTbXcDuR9tYmXJMI4vyId0FdOgBaFmcwDBnkpDBe39DTqUpnVl6pP8ZPds6Jq%2B1LJCKqB%2BofdPiuO3tEnbb8B5aNfK98IRDKAFTVqWo4nUPwrK%2BvK82Tle7hVe0%2F9UA%2BiK4A2D8XPCXt8hnoME2t7jGzZIqgSas6cZzuAGbOr0VXhP262wM7nPGqmRLjiKgDYpvBCRG%2FVUbbMMyS16cGOrEBX7Z%2BnVp%2BfxhBPYMrFziUwvziPawWX6NGlXnfQ8lh6SUIMmisjrY%2BBr84XYUR%2FslIJC8xcQisa3Z6MApovdO96SL52lYEcDBljXQYH0Q63UsRHyyG3kDnWcXLrpi6qHA5A%2BKe%2ForXAnFS13OlXNmeTSBBaqiL04Wbbfi%2FQpIFuJ0O5lvSujOV0xHzY%2BWZEpm96Ff7do3hpFaHK3e348skp5P4iHKVVch%2Fx%2FztYvAxqTAb&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20230904T132251Z&X-Amz-SignedHeaders=host&X-Amz-Expires=300&X-Amz-Credential=ASIAQ3PHCVTYRDDEVVG2%2F20230904%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=819e39a2afbde0b0d6dec77658d23f8e40fb16dd6e89a601369b8b93fce6a0c3&hash=2f3a5b2e0dddf023be64cd2fdb68bed8ee7cb90a6d4a5c4c309cc1de2e3fffb9&host=68042c943591013ac2b2430a89b270f6af2c76d8dfd086a07176afe7c76c2c61&pii=S0893608020302197&tid=spdf-fecb45fc-731d-4707-b4e0-cf4a6a46a1c4&sid=ffa363649cda874be43b0b855c8d221bcb32gxrqb&type=client&tsoh=d3d3LnNjaWVuY2VkaXJlY3QuY29t&ua=1308585355585f0c565700&rr=80168b30be601bc8&cc=in
- https://arxiv.org/pdf/2003.03253.pdf











- Tensorflow
file:///C:/Users/pushk/Downloads/1603.04467v2.pdf


## Source

- https://www.researchgate.net/profile/Vladimir-Nasteski/publication/328146111_An_overview_of_the_supervised_machine_learning_methods/links/5c1025194585157ac1bba147/An-overview-of-the-supervised-machine-learning-methods.pdf
%% - https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=990138 %%
- https://link.springer.com/content/pdf/10.1007/s11277-017-5224-x.pdf
- https://www.tandfonline.com/doi/epdf/10.1080/01431160600746456?needAccess=true&role=button
- https://arxiv.org/pdf/1511.08458.pdf
- CNN Mathematical Implementation -> https://cs.nju.edu.cn/wujx/paper/CNN.pdf
- https://arxiv.org/pdf/1912.05911.pdf RNN to be completed
- https://hal.science/hal-01352061/file/publishedversion.pdf