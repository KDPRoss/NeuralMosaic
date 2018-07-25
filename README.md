# NeuralMosaic

NeuralMosaic is a Python + TensorFlow project
aimed at generating a photomosaic from an input
image. It uses the VGG-19 CNN to project image
tiles into a 'semantic space', then it finds the
'best' image from the library for each tile of the
input image via cosine similarity.

## Prerequisites for Running the Code

- Python (3, obviously ... it's 2018, and there's
  no excuse to still be using 2)

- TensorFlow, Numpy, Scipy, Matplotlib

- VGG-19 weights file

- some library of small, square images

- an input image

- decent amount of RAM

- half-decent GPU wouldn't hurt

## Project Structure

The code is in `NeuralMosaic.ipynb`, a Jupyter
notebook, which explains the remainder of the
details.

The `examples` directory shows a number of matched
input-output pairs; except for the Monet painting,
the photographs there are all my work. I tried to
choose a variety of images to illustrate how the
code handles detailed images, colour gradients,
greyscale images, etc. The output files have a
suffix `m_n` indicating which layer of the CNN was
used to generate feature vectors.

The output-tile library that I used was ~120k
32x32 images, I generated `.png` output from the
Python code and compressed to `.jpg` in Photoshop.

Some observations:

- `monterey-wharf`: The sky has a gentle gradient,
  which is rendered using highly-repeated sky-blue
  tiles. Given the image library, solid-colour or
  gentle gradients simply don't have many similar
  tiles to choose from, so this isn't entirely
  surprising; the ripples on the water, however,
  show a vastly-more-interesting variety.
  ![Image of Wharf](https://github.com/KDPRoss/NeuralMosaic/raw/master/examples/monterey-wharf-photomosaic4_2.jpg)

- `leopard-shark`: This greyscale image produced
  *dreadful* results using earlier CNN layers; I'm
  not sure how many greyscale(ish) (and by 'ish',
  I mean 'low-saturation') images there are in my
  tileset, but I know that it's a tiny minority;
  using later layers of the network -- when
  more-abstract features are encoded -- seems to
  allow the code to focus on finding tiles that
  match up with the textures, luminosity, etc.
  ![Image of Shark](https://github.com/KDPRoss/NeuralMosaic/raw/master/examples/leopard-shark-photomosaic5_4.jpg)

- `azureus`: This is the output from the examples
  that I find most-compelling. The
  intermediate-level of detail on the frog, rocks,
  and leaves seems particularly amenable at the
  tile size / other parameters that I'm using;
  it's not *quite* able no match up the spots on
  the frog's back, but it's clearly 'tried' (i.e.,
  a number of the spots have been replaced by
  tiles that decently match their rough shapes).
  ![Image of Pretty Frog](https://github.com/KDPRoss/NeuralMosaic/raw/master/examples/azureus-photomosaic4_2.jpg)

- `central-coast-fields`: Here, again, there is a
  lot of repetition in the sky's tiles; the trees,
  however, in the midground seem to have been
  tiled quite well.
  ![Image of Fields with Blue Sky](https://github.com/KDPRoss/NeuralMosaic/raw/master/examples/central-coast-fields-photomosaic4_2.jpg)

- `jardin-des-plantes`: Here, I've upscaled a low
  depth-of-field SLR image, generated the
  photomosaic, then downscaled; it's mostly chosen
  'pixels' (i.e., nearly-solid-colour) tiles, but
  it's done relatively well on the detail in the
  foreground.
  ![Image of Jardin des Plantes](https://github.com/KDPRoss/NeuralMosaic/raw/master/examples/jardin-des-plantes-photomosaic4_2-with-rescaling.jpg)

- `monet-waterlilies`: Close up, it's rather
  difficult to make out the image, but viewing
  from a distance it looks rather good ... which,
  I suppose, is what some people would say about
  the Impressionists' work.
  ![Image of Monet's Waterlilies](https://github.com/KDPRoss/NeuralMosaic/raw/master/examples/monet-waterlilies-photomosaic4_2.jpg)

## Caveats

This is a 'batteries-not-included' sort of
project: To run it, you'll need to find / convert
the pretrained CNN weights; likewise, you'll need
a library of image tiles.

Currently, the code is built on the assumption
that image tiles will be square. The code also
uses the same tile size for the input image; this
can be relaxed by rescaling the tiles of the input
image. Potentially-unpleasant cropping will occur
if the input image is not a multiple of the tile
size.
