### A brief introduction to DeepFakes and their history

Deepfakes first came into the limelight of mainstream media in 2017, when a user on GitHub named 'Deepfake', a portmanteau of "deep learning" and "fake", posted realistic looking explicit videos of celebrities. This lead to a flurry of articles from established media houses, the most recent one in memory stating that [we can no longer believe what we see](https://www.nytimes.com/2019/06/10/opinion/deepfake-pelosi-video.html). 

However, the technology behind faceswapping, which is a major portion of what deepfakes do, is not a new subject. Face detection and swapping has been a major research subject in computer vision, a subgenre of computer science, since the early 2000s, with libraries such as OpenCV being used to swap faces programmatically, as seen in [Sathya Mallick's article from 2016](https://www.learnopencv.com/face-swap-using-opencv-c-python/).

![Face swap using openCV](https://www.learnopencv.com/wp-content/uploads/2016/04/presidential-candidates.jpg)

<p align="center">
<h6>Faceswapping attempts of 2016 presidential candidates using OpenCV</h6>
</p>

This technology can only be used on pre-existing images of both faces. It cannot morph one person's face into another maintaining the exact original expression if a pre-existing picture with that expression does not exist. This is where deepfakes come in. The novelty of a deepfake is that using it, one would be able to transform person A’s face to mimic someone else’s facial features (say a person B), although preserving the original (person A's) facial expression.

When done right, this can be used to create photorealistic images (and videos). Positive outcomes from the application of this technology can be seen in examples such as a [Deepfake Dalì being used to engage customers in a museum in St. Petersburg, Florida](https://www.theverge.com/2019/5/10/18540953/salvador-dali-lives-deepfake-museum). We explore the troubles which come along with the adoption of this technology in the **Ethics** section below.

Before that, we deepdive into the machine learning under a deepfake. 

### How a deepfake works

The most simple version of a deepfake relies on the neural network known as an **autoencoder**, a deep neural network consisting of 2 components -an encoder and  adecoder. The encoder's role is to learn to encode an input image into a lower-dimensional representation, while the decoder’s role is to reconstruct this representation back into the original image which, at a high level. This lower dimensional latent space within the autoencoder acts as a bottleneck and ensures that the network actually recreates these images instead of just outputting the inputted image pixel by pixel. 

Images containing the faces of both people involved in the deepfake are passed through the autoencoder to help the latent space 
learn broader patterns within the image. For example, the latent space learns how to recreate Tobey Maguire's jawline. 

The deepfake setup has the images compressed to the lower dimension latent space for both persons involved in the deepfake and two separate decoders, one to turn the representation back to Person A (Tobey Maguire) and one to turn the representation back to Person B (Tom Holland). 
<p align="center">
![Fallon Oliver autoencoder](https://cdn-images-1.medium.com/max/800/1*3PGiPLIUEzd0ZMLxlQyFhw.png)
</p>
```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Delete before submission VVVVVV

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/siddhartha-s/spidermen-deepfake/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
