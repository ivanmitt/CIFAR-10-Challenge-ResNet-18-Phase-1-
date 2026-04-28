# CIFAR-10-Challenge-ResNet-18-Phase-1-
My modified ResNet-18 architecture that won my class's CIFAR-10 competition.

For Phase 1, we weren't allowed to use transfer learning. All the code was initially run on Google Colab using a T-4 GPU.
My methodology behind Phase 1 was to essentially "fit" the ResNet-18 architecture to the CIFAR-10 dataset; hence why I refer to it as a modifed ResNet-18. My primary concern with no transfer learning was my model overfitting to the training set.

QOL Functions:
In the initial mounting of the CIFAR-10 dataset I added a copy command for the GPU's to download the data locally, when I initially ran these models I had poor network connection and didn't want to wait for each image to be passed back and forth over the network.
Secondly, when I set the device to use the GPU, I added a failsafe to default to CPU in case the GPU's VRAM ran out.

Data Augmentation Summary:
To allow the training set to best learn from scratch, I take a 32x32 crop of each image with a 4-pixel wide padding. This forces the model to learn that the subject of the image isn't determined because it happens to be in the center of the image. Additionally, I incorporated a horizontal flip to double the dataset by using the mirror image of the CIFAR-10 dataset. Afterwords, the images are transformed into tensors and centered based off the 32x32 mean.

Label Smoothing Summary:
I set the model's label smoothing to 90% sure of the image, at 100% the model would've overfit to the training set and underperformed in the testing portion.

ResNet-18 Modifications:
There are two distinct changes I made to the standard ResNet-18 architecture that allowed my model to even surpass my entire class' Phase 2 models: a standard ResNet-18 does two things, first, it applies an early convolution to analyze the data in a 7x7 kernel with a stride of 2. Using a stride of two means the model only checks every other pixel, cutting my 32x32 images into essentially 16x16 images. Additionally, the standard MaxPool function would further cut these images in half into 8x8 images. With the CIFAR-10's images being so small (compared to ImageNet's 224x224 resolution), I wanted image integrity to be preserved into the deeper learning layers.

Learning Summary:
In class we implemented Adam optimizers to adjust weights, I used an AdamW optimizer as it's objectively better at preventing overfitting. Additionally, for Phase 1 I set a relatively high learning rate since the model needed to identify broad patterns before it could learn nuanced differences. To prevent a large, linear learning function causing my model to skip over more profound learning curves, I applied a Cosine Annealing function. This changes the learning rate to decrease over time cosinusoidally, allowing the model to settle into a local minimum. Lastly, I set the learning rate optimizer to the number of epochs being run, meaning the model took all 50 epochs to gradually optimize its weights to the training set.

Training Summary:
After 50 epochs, the model finished with an accuracy of 99.91%, each epoch taking roughly 43 seconds to train. This was expected given the model is constantly running on the same images, but I was still concerned with overfitting.

Testing Summary:
As a result, the model finished with an F-1 score of 93%, the highest for my class' Phase 1. Surprisingly, for my class' Phase 2, only two other students had an F-1 score of 93% (second place had 93.9%).

Further Optimizations:
Across all the documentation I read about ResNet-18's, I think my model did fairly well. The only adjustments I could make are more data augmentations to add to the training dataset, more epochs to learn (although this might cause overfitting), slightly adjusted learning rates, or a potentially better-suited optimizing function for the weights.
