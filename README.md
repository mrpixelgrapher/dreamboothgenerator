# Dreambooth Diffuser Guide
## _I hope this guide will be able to give you some clarity on how to make the most out of dreambooth and how to train a model that will have likeness to your training images._


Hey everyone,
My name is Prakhar Saraswat, you can connect with me on https://twitter.com/Mrpixelgrapher 

So most of the people I've interacted with are as confused as I was before I started running tests on Dreambooth using Shivam's colab which is based on diffusers implementation for dreambooth base on diffusers from hugging space.

Link to Shivam's colab: https://colab.research.google.com/github/ShivamShrirao/diffusers/blob/main/examples/dreambooth/DreamBooth_Stable_Diffusion.ipynb


If you want TL;DR version, these are the settings that worked for me and can act as a good starting point for you:
- Instance name: [Any Token Name]
- Class Name: Man
- num_class_images/Reg img no.= 20
- max_train_steps= 1600
- custom DDIM images as reg images


BONUS TIP: Use this tool to copy your models from one google drive account to another (backup drive): https://script.google.com/macros/s/AKfycbxbGNGajrxv-HbX2sVY2OTu7yj9VvxlOMOeQblZFuq7rYm7uyo/exec?pli=1


Before we get into the results and comparison of different models that I have trained, let's start with basic nomenclature of terms so that this guide makes more sense. Once, you open the Colab you are greeted with terms that might be confusing and it's hard to assimilate on how they will affect your model per se. 

------
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

![Capture](https://user-images.githubusercontent.com/113246464/194565511-4f71bf25-987d-47fe-bf93-e3ec4c428310.PNG)

These were the iimages from the first model that I trained. In the above images you can see a burned up look, which is a result of overtrianing.
There are a lot of artifacts and the amount of poses is limited to that of the training image. Soemtimes, even copies of your subject shows up.

To understand how to stop overtraining it was important to figure out the right number of steps and the number of training images, I ran these tests to find out:

## Test 1: > Number of training images to be used?

So I created three training sets, and trained models (1,2,3 respectively) on:
- 1st training set containing 6 images (2 headshots, 2 half body shots and 2 full body shots)
- 2nd training set containing 24 images (10 headshots, 8 half body shots, 6 full body shots)
- 3rd training set containing 20 images (4 headshots, 11 half body shots, 5 full body shots)

These settings for these models were kept constant:

- Instance name: Prakhar1/2/3
- Class Name: People
- Reg img no.: 12


The results from these tests were tested against these prompts:
```sh
1. portrait of {_}, 8k
2. Film still of {_} as doctor strange in avengers endgame
3. Incredible portrait of {_}, artstation winner by Victo Ngai, Kilian Eng and by Jake Parker, swirly vibrant color lines, winning-award masterpiece, fantastically gaudy, aesthetic octane render, 8K HD Resolution
4. half body portrait of {_} in a red black tuxedo, highly detailed, digital painting, art by greg rutkowski, 8k
5. {_}, white and multicolored hair, surrounded by flowers, cosmic background, realistic shaded perfect face, fine details by realistic shaded lighting poster by ilya kuvshinov katsuhiro otomo, magali villeneuve, artgerm, jeremy lipkin and michael garmash and rob rey
6. portrait of {_}, aesthetic octane render, 8K HD Resolution
```
NOTE: Only the best result from the 10 generations per prompt is being showcased! Most of the images from first and second model were mediocre. 

## The results from the first model:

Training steps: 800
Training images: 6

 
![Capture](https://user-images.githubusercontent.com/113246464/194619503-a5da6ab1-4b84-4161-a9e5-42a53d3d01b5.PNG)

This model's generations are decent coherence wise, good for digital artstyle type of results but not that great when it comes to photorealistic results. The training time was really less with just 800 steps and 6 images, showing that you can get decent results even with a smaller training set, though the face profile and composition of the generation frames in between different prompts, still looks nearly similar.


## The results from the 2nd model:

Training steps: 2020
Training images: 24


![Capture](https://user-images.githubusercontent.com/113246464/194621047-9df821f2-40ef-41cc-ad58-205dcd2bea1b.PNG)

This model was pretty versatile in the type of different compositions that were generated and has really good detail level, but in a lot of ways the face structure between different style wasn't coherent. At best this model had a sense of my resemblance but had too many training images with so much variety that it understood my face structure as maybe not a fixed thing.


## The results from the 3rd model:

Training steps: 1500
Training images: 20


![Capture](https://user-images.githubusercontent.com/113246464/194624546-fcf0eedc-4b03-4e53-a5af-841e186f76b7.PNG)

Oh boy! This turned out darn good, with usually really good resemblance to me in all the different prompts as well as different compositions. It was good to know at this point that 20 training set and it's composition was the way to go.
