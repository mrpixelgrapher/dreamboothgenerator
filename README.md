# Dreambooth Diffuser Guide
## _I hope this guide will be able to give you some clarity on how to make the most out of dreambooth and how to train a model that will have likeness to your training images._


Hey everyone,
My name is Prakhar Saraswat, you can connect with me on https://twitter.com/Mrpixelgrapher 

So most of the people I've interacted with are as confused as I was before I started running tests on Dreambooth using Shivam's colab which is based on diffusers from hugging space.

Link to Shivam's colab: https://colab.research.google.com/github/ShivamShrirao/diffusers/blob/main/examples/dreambooth/DreamBooth_Stable_Diffusion.ipynb



Before we get into the results and comparison of different models that I have trained, let's start with basic nomenclature of terms so that this guide makes more sense. Once, you open the Colab you are greeted with terms that might be confusing and it's hard to assimilate on how they will affect your model per se. 

## Important terms to understand this guide:

- 1. INSTANCE_DIR: This is the directory where you model will download all the dependencies that it needs to run.
- 2. OUTPUT_DIR: is the drive location where your model will get saved.
- 3. Weights: a diffusion based DB model egenrates a weights file that is used to generate images using SD.
- 4. Checkpoint/CKPT: is a checkpoint file that is used as a model to run SD in WEBui such as automatic1111 or in deforum notebook.
- 5. Class name: is used as a "class" that we will use to train our model and associate our token with. For eg if our training subject is a man, this will be man/guy/person.
- 6. Token: Is the term that we use to define our subject or instance name. (In all of my experiments the instance name/token name are the same) Hence, these terms might be used interchangeably in this guide.
- 7. Traning images: The images you feed into DB so that it understands you subject properly and can replicate it in different ways using prompts.
- 8. Reg/regularisation Images: These are the images your model trains you training image against to define what your class should look like and your images in it.
- 9. Steps: The number of steps or in lay man terms scans, that wil be done with your training images to understand the likeliness of your subject.
- 10. Over training: When your model generates burned up images.


## Understanding settings of Colab for training.

![Capture](https://user-images.githubusercontent.com/113246464/194551917-9d3e94c3-6bd0-40d8-ade2-163e0f81f3c1.PNG)
(This is a Screenshot of the settings in colab, now we will try to decipher what settings we will be changing in these experiments and what effect they'll have:


- 1. num_class_images: This controls the number of reg images that the model will count. You can either let the Colab generate them by default or manually upload these images. It is necessary to mention the exact count of reg images here because if you specify "20" here and upload only 10 custom reg images, then system will generate 10 reg images on it's own to reach the "20" count.
- 2. max_train_steps: This controls the number of steps. You can easily decide on your number of steps by deciding the number of scan cycles (number of images x number of cycles = steps). On an aveage one should target a minimum of 50-80 cycles. We will look into this during the experiments.
- 3. instance_prompt= is used to define your token/instance for a comparison with class prompt.
- 4. class_prompt: This is used as the prompt to generate the reg images.
- 5. seed: This is the seed setting that will be used by default to generate reg images based on your class prompt.


## What does overtraining look like?

![m (19)](https://user-images.githubusercontent.com/113246464/194556438-15091a9f-2cac-4599-ad3f-6f789a2c145a.png)
![download (6)](https://user-images.githubusercontent.com/113246464/194556591-d74a4eb3-90b9-4141-a3a3-21858f3d86c3.png)

In the above images you can see a burned up look, which is a result of overtrianing.

To understand at what steps our model should run, what should be the number of training images, I ran these tests:

> Test 1
> Number of steps to be used?


