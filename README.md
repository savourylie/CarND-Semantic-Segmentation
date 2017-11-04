# Semantic Segmentation
## Abstract
This is a Udacity coursework project from Term 3 of the Self-driving Car Nanodegree program.  The task is to tell roads apart from the rest of the stuff in the image for the self-driving car to know where the valid track is for it to drive on. Unlike the usual bounding box method, in this project we'd like to correctly mark the roads down to the pixel level so that it is more precise and can work with more situations.

## Method
In this project we use a Fully Convolutional Network (FCN) which consists of an encoder and a decoder. The encoder part is a pretrained VGG16 network, attached to which we use a one by one convolutional layer to replace the usual fully connected classifier. The two operations are element-wise identical only that the convolution version preserves the spacial information which is essential for our task. Then through a few transpose convolutions and upsamplings we get back to the same image dimension as we started with, with the right road pixels marked. There is one extra trick worth mentioning, the skip connections. While convolution and pooling layers help us extract the most essential information for our task, down through them some information are bound to be lost. Skip connections can help us recover the lost information by making extra connections between several none-adjacent layers so that information that are lost between the layers are recovered through the skip connections.

## Result
The hyperparameter tuning isn't too tricky in this project. However, I did start with a learning rate that is way too small (1e-6), and it took a while for me to realize that I just had to increase it by a lot so that I can train the FCN well in a reasonable time.

I trained the FCN on my local machine with two 1070s and two 1060s. I didn't time it but it felt like each epoch took about 30 seconds. On Slack a lot of people suggest that we should go with a small batch size, about 5 ~ 10, which didn't quite make sense to me. However, I did find that it took much longer to train the network with `batch_size = 64`. I'm not sure why this is the case and I took a mental note to to dig further into this later.

In the end I went with `batch_size = 16` and `epochs = 160` and I'm happy with result.

The result are saved in the `run` folder.

## How to use
Simply run `python main.py`  and it'll start training on the training set. Once the training is done, it'll start predicting on the test set and a new set of prediction images will be generated in the `run` folder.