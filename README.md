### A brief introduction to DeepFakes and their history

Deepfakes first came into the limelight of mainstream media in 2017, when a user on GitHub named 'Deepfake', a portmanteau of "deep learning" and "fake", posted realistic looking explicit videos of celebrities. This lead to a flurry of articles from established media houses, the most recent one in memory stating that [we can no longer believe what we see](https://www.nytimes.com/2019/06/10/opinion/deepfake-pelosi-video.html).

However, the technology behind faceswapping, which is a major portion of what deepfakes do, is not a new subject. Face detection and swapping has been a major research subject in computer vision, a subgenre of computer science, since the early 2000s, with libraries such as OpenCV being used to swap faces programmatically, as seen in [Sathya Mallick's article from 2016](https://www.learnopencv.com/face-swap-using-opencv-c-python/). Further, image and video manipulation software has seen widespread use for over a decade. The big innovation offered by deepfakes is automation; the formerly time-consuming process of using editing software can be performed without human input.

![Face swap using openCV](https://www.learnopencv.com/wp-content/uploads/2016/04/presidential-candidates.jpg)

<p align="center">
<h6>Faceswapping attempts of 2016 presidential candidates using OpenCV</h6>
</p>

This technology can only be used on pre-existing images of both faces. It cannot morph one person's face into another maintaining the exact original expression if a pre-existing picture with that expression does not exist. This is where deepfakes come in. The novelty of a deepfake is that using it, one would be able to transform person A’s face to mimic someone else’s facial features (say a person B), although preserving the original (person A's) facial expression.

When done right, this can be used to create photorealistic images (and videos). Positive outcomes from the application of this technology can be seen in examples such as a [Deepfake Dalì being used to engage customers in a museum in St. Petersburg, Florida](https://www.theverge.com/2019/5/10/18540953/salvador-dali-lives-deepfake-museum). We explore the troubles which come along with the adoption of this technology in the **Ethics** section below.

Before that, we dive into the machine learning under a deepfake.

### How a Deepfake Works

The most simple version of a deepfake relies on the neural network known as an **autoencoder**, a deep neural network consisting of 2 components--an encoder and a decoder. The encoder's role is to learn to encode an input image into a lower-dimensional representation, while the decoder’s role is to reconstruct this representation back into the original image. This lower-dimensional latent space within the autoencoder acts as a bottleneck and ensures that the network actually recreates these images instead of just outputting the inputted image pixel by pixel.

In a successfully trained autoencoder, the latent space encodes enough information to send arbitrary points through the decoder to create novel images. In some cases such as variational autoencoders, a special loss function is used to impose structure on the latent space to encourage points in the latent space that do not correspond to training points to produce reasonable outputs. In other cases, such as ours, we train a single encoder but multiple decoders so that we can decode points that were used to train the encoder but not the decoder.

### Ethics

Although creating a representation of something that does not exist in reality, even a photorealistic representation, is not a new practice, deepfakes offer unprecendented access and efficiency to their creation. Deepfakes have already been used in ethically irresponsible ways, such as the aforementioned faceswapping the faces of celebrities into explicit videos.

This is not to condemn the technology; there have been legitimate reasons to go through the effort of creating fake content (such as entertainment). They remain the same and are enhanced by deepfakes. However, as always happens when barriers to entry of a powerful technology are removed, the potential abuses are greatly magnified. People are already working on deepfake detection algorithms. It is the responsibility of those of us who understand and can use the technology to minimize the potential for abuse.

### Spiderman Swapping

As we looked through libraries and tutorials for faceswappers and autoencoders we noticed that faceswappers are generally packaged and as such hide the mechanics of the software from the user. Simple autoencoders are often used as tutorials, but they almost always use the MNIST handwritten digit dataset. While this is informative, the MNIST dataset is so simple that some important aspects of real-world use are glossed over. For example, most work with images is done with 3 channels (RGB) rather than single channel (grayscale), but MNIST digits are single-channel images. Further, they are such small images that autoencoders will use at most one intermediate step between the input layer and the latent representation layer.

We attempt to address these issues by creating a real-world example of an autoencoder replacing one famous Spiderman actor with another. We collected thousands of images of Tom Holland and Tobey Maguire from YouTube videos and created an autoencoder with images of their faces. We also created a decoder trained specifically with Tom Holland pictures and the same encoder from the first autoencoder. Then an image of Tobey Maguire can be passed through the encoder and Tom Holland decoder to create a Tom Holland version of whatever facial expression Tobey Maguire is making. For more details of network architecture, see the notebooks in this repository.
