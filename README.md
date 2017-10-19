# Style Transfer as Optimal Transport 

## This is a [Tensorflow](https://github.com/tensorflow/tensorflow) implementation of an algorithm that transfers the distribution of visual characteristics, or style, of one image onto a second image via an [Optimal Transport](https://en.wikipedia.org/wiki/Transportation_theory_(mathematics)) plan.

<img src="ipynb_example/kanagawa_surf.png" width="960"/>

- - - - 

### Table of Examples. To see detail in 1080x1920 resolution, please click through the image thumbnails and select download. There is a lot of detail. 
<p float="right">

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<img src="styles/crazycat.jpg" width="192" height="150"/>
<img src="styles/matrix.jpg" width="192" height="150"/>
<img src="styles/starrynight.jpg" width="192" height="150"/>
</p>

<p float="left">

<img src="subjects/city.png" width="192">
<img src="output/cat_city.png" width="192"/>
<img src="output/matrix_city.png" width="192"/>
<img src="output/vangogh_city.png" width="192"/>
</p>

<p float="left">

<img src="subjects/joey.png" width="192">
<img src="output/cat_joey.png" width="192"/>
<img src="output/matrix_joey.png" width="192"/>
<img src="output/vangogh_joey.png" width="192"/>
</p>

<p float="left">

<img src="subjects/lion.png" width="192">
<img src="output/cat_lion.png" width="192"/>
<img src="output/matrix_lion.png" width="192"/>
<img src="output/vangogh_lion.png" width="192"/>
</p>

- - - - 

## Files:
[example.ipynb](example.ipynb) - notebook that demonstrates use case, the output pictures and video can be found in [ipynb_example](/ipynb_example)

[theory.ipynb](theory.ipynb) - illustrates problems with gatys loss function (frobenius norm of difference in Graham matrices) and provides calculations of the computational intractability of full Earth Mover's distance. 

[vgg.py](vgg.py) - script that unpacks 'imagenet-vgg-verydeep-19.mat' found [here](https://github.com/anishathalye/neural-style) | author @anishathalye

[synthesize.py](synthesize.py) - synthesizes an image that represents a 'subject' image with the visual characteristics of a 'style' image 


- - - - 

### Algorithm Overview - sections correspond to [synthesize.py](synthesize.py)
1. `get_style_desc`: A 'style' image is fed into the [vgg network](https://arxiv.org/pdf/1409.1556.pdf), which maps RGB-pixel values through a series of feature spaces (or layers) calibrated to provide discriminatory information for an image classification engine. The dimensionality of the feature space grows from 3 colors (red, green, and blue) to 64, 128, 256, and eventually 512 abstract features ([good visualizations](http://yosinski.com/deepvis)). Here, we conceptualize each pixel at a specific layer as a realization of a random vector; for example, if an image is represented as a tensor of shape 100x100x128 (height x width x feature activations) this would be 10,000 realizations (or samples) of a 128 dimension vector. The first two moments (mean vector and covariance matrix) are inferred empirically and stored as a representation of the style.* 

2. `infer_loss`: A 'subject' image is fed into the same network and the first two moments of the activations are calculated in an identical manner. The [L2-Wasserstein distance](https://en.wikipedia.org/wiki/Wasserstein_metric) between these parametrized distributions is used as a loss function. 

3. `scipy_optimizer` or `build_image`: An optimizer is invoked (either [l-BFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) via scipy or [Adam](https://arxiv.org/abs/1412.6980) native to Tensorflow) to modify the subject image such that the distance between the parametrized distributions is minimized. 

*Note: Considering only the first two moments of the activations implicitly assumes the random vectors/feature activations are [multivariate-Gaussians](https://en.wikipedia.org/wiki/Multivariate_normal_distribution). 

Currently a work in progress.. please stay tuned for updates

Let me know if you are interested in more explanation/theory behind this formulation. If there is interest, I will write it up formally.


- - - - 

## To do:
 - add derivation of Wasserstein distance for multivariate gaussian to theory.ipynb
 - include empirical analysis of assuming gaussian structure - how violent is this assumption?
 
- - - - 

## Requirements: 

Utilizes python packages: tensorflow, numpy, scipy.io, PIL.Image, time

Tested using tensorflow 1.3, python 3.5

Must have the .mat file containing vgg network weights and biases in parent directory, can be found: http://www.vlfeat.org/matconvnet/models/beta16/imagenet-vgg-verydeep-19.mat ((MD5 8ee3263992981a1d26e73b3ca028a123)


See [example.ipynb](example.ipynb) for use case. 


