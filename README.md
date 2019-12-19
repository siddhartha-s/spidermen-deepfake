### A brief introduction to DeepFakes and their history

Deepfakes first came into the limelight of mainstream media in 2017, when a user on GitHub named 'Deepfake', a portmanteau of "deep learning" and "fake", posted realistic looking explicit videos of celebrities. This lead to a flurry of articles from established media houses, the most recent one in memory stating that [we can no longer believe what we see](https://www.nytimes.com/2019/06/10/opinion/deepfake-pelosi-video.html).

However, the technology behind faceswapping, which is a major portion of what deepfakes do, is not a new subject. Face detection and swapping has been a major research subject in computer vision, a subgenre of computer science, since the early 2000s, with libraries such as OpenCV being used to swap faces programmatically, as seen in [Sathya Mallick's article from 2016](https://www.learnopencv.com/face-swap-using-opencv-c-python/). Further, image and video manipulation software has seen widespread use for over a decade. The big innovation offered by deepfakes is automation; the formerly time-consuming process of using editing software can be performed without human input.

![Face swap using openCV](https://www.learnopencv.com/wp-content/uploads/2016/04/presidential-candidates.jpg)

<p align="center">
<h6>Faceswapping attempts of 2016 presidential candidates using OpenCV</h6>
</p>

This technology can only be used on pre-existing images of both faces. It cannot morph one person's face into another maintaining the exact original expression if a pre-existing picture with that expression does not exist. This is where deepfakes come in. The novelty of a deepfake is that using it, one would be able to transform person A’s face to mimic someone else’s facial features (say a person B), although preserving the original (person A's) facial expression.

When done right, this can be used to create photorealistic images (and videos). In spite of the potential dangers of this technology, there are many positive examples of its use. In a fun, interactive exhibit, an ad agency created a [Deepfake Salvador Dalì engage with museumgoers in St. Petersburg, Florida](https://www.theverge.com/2019/5/10/18540953/salvador-dali-lives-deepfake-museum).


### How a Deepfake Works

#### Autoencoders

The most simple version of a deepfake relies on the neural network known as an [**autoencoder**](https://towardsdatascience.com/auto-encoder-what-is-it-and-what-is-it-used-for-part-1-3e5c6f017726), a deep neural network that can be separated into 2 components--an **encoder** and a **decoder**. An autoencoder's goal is to process an image and give you back the same image as closely as it can recreate it. The tricky part is that the autoencoder has to throw away information in the encoder half and try to interpolate what it threw away in the decoder half.

We will take the example of an image. An image is usually defined by thousands of individual pixels, which are generally defined by 3 integers from 0 to 255 representing the relative strength of red, green, and blue in the image. If your image is as small as 256x256, this gives the any neural net that reads it a 196,608-dimensional input! An autoencoder is a neural net that starts out with an input layer of a certain size, such as the 196,608 dimensions of our picture, with each subsequent layer getting smaller until a smallest layer in the middle called the **bottleneck layer**. After the bottleneck layer, the dimensionality of each layer progressively increases up to the output layer, which has the same dimensionality as the input layer. The network is trained to minimize the difference between the input and output layers.

![Autoencoder example](https://miro.medium.com/max/1440/1*gZCVcwRz0ziWdoQL3VzSAw.png)

So if the network learns to output the same thing as was put in, why bother making a network at all? The answer lies in the **latent space**, a term for vectors of the dimensionality as the bottleneck layer. By isolating the half of the neural network up to the bottleneck, information can be "encoded" in the latent space. This encoded vector in the latent space has value because it is smaller than the original image but can be "decoded" by the second half of the neural network to output something that (hopefully) looks very similar to the image that was put into the encoder. Because of this, the value in an autoencoder is the separated ability to encode and decode with its halves.

#### Generation with Autoencoders

As useful as it is to have an encoder that compresses information down to a lower-dimensional representation and a decoder that can create an image from the encoding, those alone do not give the ability to create deepfakes. Novel image generation can occur, however, because of one important fact: the decoder can be used on any vector in the latent space, not just a real image's vector encoding.

However, throwing randomly generated vectors from the latent space into the decoder is unlikely to give meaningful output. Most vectors in the latent space will generate images of useless noise when decoded, but there are some ways around this. [**Variational Autoencoders**](https://towardsdatascience.com/intuitively-understanding-variational-autoencoders-1bfe67eb5daf) try to enforce a meaning to proximity in the latent space. In other words, vectors that are close to each other in the latent space of a variational autoencoder should produce similar output, which may or may not happen in another autoencoder. They impose this structure on the latent space by way of the loss function. A normal autoencoder tries to minimize the difference between the input and output images, but a variational autoencoder learns both a mean and a variance in the latent space. With those numbers and the assumption that a Gaussian distribution exists, variational autoencoders can generate novel images from points in the latent space.

It is also possible to train multiple decoders off a single encoder. To do this, begin with a training set and train a full autoencoder. After you have that, keep the encoder and use it to find the latent space representations of a different, narrower training set. Use those to train a decoder specific to that narrower category of data, and then anything from the broader training set can be encoded with your encoder and decoded with a more specific decoder that will transform it into something resembling the narrower class of data.

That is all very broad, so we will examine a [specific example](https://goberoi.com/exploring-deepfakes-20c9947c22d9).

![Multiple decoder case](https://miro.medium.com/max/1182/1*3PGiPLIUEzd0ZMLxlQyFhw.png)

In the above picture, an autoencoder was trained on a set of distorted faces of Jimmy Fallon and John Oliver. That encoder was used to train a decoder with only data from Fallon and another exclusively of pictures of Oliver.

![Fallon to Oliver transformation](https://miro.medium.com/max/1182/1*3m1BKsjoOavu4UPcZw4-Iw.png)

The decoder above was trained only with pictures of John Oliver, so it will generate images of his face. However, the encoder was trained to encode pictures of both Fallon and Oliver, so sending a picture of Fallon into the encoder and decoding it as a picture of Oliver yields a picture of Oliver retaining characteristics of the input Fallon face.


### Spiderman Swapping

#### Why swap Spidermen?

As we looked through libraries and tutorials for faceswappers and autoencoders we noticed that faceswappers are generally packaged and as such hide the mechanics of the software from the user. This is useful but uninformative. On the other hand, simple autoencoders are often used as tutorials, but they almost always use the popular MNIST handwritten digit dataset. While this is more informative, the MNIST dataset is so simple that some important aspects of real-world use are glossed over. For example, most work with images is done with 3 channels (RGB) rather than single channel (grayscale), but MNIST digits are single-channel images. Further, they are such small images that autoencoders will use at most one intermediate step between the input layer and the latent representation layer. In real application, autoencoders will often perform poorly with only a single layer between the input and the bottleneck.

![Pictures](https://i.imgur.com/mTzQcO0.png)

We attempt to address these issues by creating a real-world example of an autoencoder replacing one famous Spiderman actor with another. We collected thousands of images of Tom Holland and Tobey Maguire from YouTube videos and created an autoencoder with images of their faces.

#### How are the Spidermen swapped?

Our architecture is similar to the architecture described above of Jimmy Fallon and John Oliver, though the code was not shared in that example. We started with an autoencoder trained with ~3,000 images each of Tobey Maguire's and Tom Holland's faces. We took that encoder and trained a Tom Holland-specific decoder by holding the encoder constant but training a new decoding half of the neural net and optimizing its weights to produce images of Tom Holland. The end result is that a picture of Tobey Maguire can be run through the encoder and Tom Holland decoder to produce a picture of Tom Holland's face making the same expression as the inputted Tobey Maguire picture.

![Spider swap](https://i.imgur.com/0HOKp9E.png?2)

#### Limitations

Our approach was limited by our compute power, training set, and time.

Although our code is included, we did not have time to make it fully human-readable, so savvier readers intending to learn from our network architecture will not have as easy of a time as we had intended.

The faceswapper also performed poorly on images taken from outside its fairly narrow intended use case of transforming Tobey Maguire (or Tom Holland) into Tom Holland. Below is its attempt to transform our faces into Tom Holland's:

![Faceswap fail](https://i.imgur.com/pQ1tqQh.png?1)
