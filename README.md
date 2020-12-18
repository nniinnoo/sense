<div align="center">

<img src="https://raw.githubusercontent.com/TwentyBN/sense/master/docs/imgs/temp_sense_header.png" height="70px">

**State-of-the-art Real-time Action Recognition**

---

<!-- TABLE OF CONTENTS -->
<p align="center">
  <a href="https://www.20bn.com/">Website</a> •
  <a href="https://medium.com/twentybn/towards-situated-visual-ai-via-end-to-end-learning-on-video-clips-2832bd9d519f">Blogpost</a> •  
  <a href="#getting-started">Getting Started</a> •
  <a href="#build-your-own-classifier">Build Your Own Classifier</a> •
  <a href="#ios-deployment">iOS Deployment</a> •
  <a href="https://20bn.com/products/datasets">Datasets</a> •   
  <a href="https://20bn.com/licensing/sdk/evaluation">SDK License</a>
</p>

<!-- BADGES -->
<p align="center">
    <a href="https://20bn.com/">
        <img alt="Documentation" src="https://img.shields.io/website/http/20bn.com.svg?down_color=red&down_message=offline&up_message=online">
    </a>
    <a href="https://github.com/TwentyBN/sense/master/LICENSE">
        <img alt="GitHub" src="https://img.shields.io/github/license/TwentyBN/sense.svg?color=blue">
    </a>
    <a href="https://github.com/TwentyBN/sense/releases">
        <img alt="GitHub release" src="https://img.shields.io/github/release/TwentyBN/sense.svg">
    </a>
    <a href="https://github.com/TwentyBN/sense/blob/master/CODE_OF_CONDUCT.md">
        <img alt="Contributor Covenant" src="https://img.shields.io/badge/Contributor%20Covenant-v2.0%20adopted-ff69b4.svg">
    </a>
</p>

</div>

---

<!-- Add some bullet points for what this repo provides-->
`sense`is an inference engine to serve powerful neural networks for action recognition, with a low
 computational footprint. In this repository, we provide: 
 - Two models out-of-the-box pre-trained on millions of videos of humans performing 
 actions in front of, and interacting with, a camera. Both neural networks are small, efficient, and run smoothly in real time on a CPU.
- Demo applications showcasing the potential of our models: gesture recognition, fitness activity tracking, live
 calorie estimation.
- A pipeline to train your own custom classifier on top of our models with an easy-to-use script to fine-tune our weights. 

<div align="center">

###### Gesture Recognition

<p align="center">
    <img src="https://raw.githubusercontent.com/TwentyBN/sense/master/docs/gifs/gesture_recognition_1.gif" width="300px">
    <img src="https://raw.githubusercontent.com/TwentyBN/sense/master/docs/gifs/gesture_recognition_2.gif" width="300px">
</p>

*(full video can be found [here](https://drive.google.com/file/d/1G5OaCsPco_4H7F5-s6n2Mm3wI5V9K6WE/view?usp=sharing))*


###### Fitness Activity Tracker and Calorie Estimation

<p align="center">
    <img src="https://raw.githubusercontent.com/TwentyBN/sense/master/docs/gifs/fitness_tracking_1.gif" width="300px">
    <img src="https://raw.githubusercontent.com/TwentyBN/sense/master/docs/gifs/fitness_tracking_2.gif" width="300px">
</p>

*(full video can be found [here](https://drive.google.com/file/d/1f1y0wg7Y1kpSBwKSEFx1TDoD5lGA8DtQ/view?usp=sharing))*

</div>

<!-- 
## From the Community 
-->
<!-- Projects from the community -->

---

## Requirements and Installation
The following steps are confirmed to work on Linux (Ubuntu 18.04 LTS and 20.04 LTS) and macOS (Catalina 10.15.7).

#### Step 1: Clone the repository
To begin, clone this repository to a local directory of your choice:
```
git clone https://github.com/TwentyBN/sense.git
cd sense
```

#### Step 2: Install Dependencies
We recommended creating a new virtual environment to install our dependencies using 
[conda](https://docs.conda.io/en/latest/miniconda.html) or [`virtualenv`](https://docs.python.org/3/library/venv.html
). The following instructions will help create a conda environment. 

```shell
conda create -y -n sense python=3.6
conda activate sense
```

Install Python dependencies:

```shell
pip install -r requirements.txt
```

Note: `pip install -r requirements.txt` only installs the CPU-only version of PyTorch.
To run inference on your GPU,  another version of PyTorch should be installed. For instance:
```shell
conda install pytorch torchvision cudatoolkit=10.2 -c pytorch
``` 
See all available options [here](https://pytorch.org/).

#### Step 3: Download the Pre-trained Weights
Pre-trained weights can be downloaded from [here](https://20bn.com/licensing/sdk/evaluation). Follow the 
instructions there to create an account and download the weights. Once downloaded, unzip the folder and move the 
folder named `backbone` into `sense/resources`. In the end, your resources folder structure should look like
 this:

```
resources
├── backbone
│   ├── strided_inflated_efficientnet.ckpt
│   └── strided_inflated_mobilenet.ckpt
├── fitness_activity_recognition
│   └── ...
├── gesture_detection
│   └── ...
└── ...
```

Note: The remaining folders in `resources/` will already have the necessary files -- only `resources/backbone` 
needs to be downloaded separately. 

--- 

## Getting Started
To get started, try out the demos we've provided. Inside the `sense/scripts` directory, you will find 3 Python scripts,
`run_gesture_recognition.py`, `run_fitness_tracker.py`, and `run_calorie_estimation.py`. Launching each demo is as
 simple as running the script in terminal as described below. 

#### Demo 1: Gesture Recognition

`scripts/run_gesture_recognition.py` applies our pre-trained models to hand gesture recognition.
30 gestures are supported (see full list 
[here](https://github.com/TwentyBN/sense/blob/master/sense/downstream_tasks/gesture_recognition/__init__.py)).

Usage:
```shell
PYTHONPATH=./ python scripts/run_gesture_recognition.py
```


#### Demo 2: Fitness Activity Tracking

`scripts/run_fitness_tracker.py` applies our pre-trained models to real-time fitness activity recognition and calorie estimation. 
In total, 80 different fitness exercises are recognized (see full list 
[here](https://github.com/TwentyBN/sense/blob/master/sense/downstream_tasks/fitness_activity_recognition/__init__.py)).

Usage:

```shell
PYTHONPATH=./ python scripts/run_fitness_tracker.py --weight=65 --age=30 --height=170 --gender=female
```

Weight, age, height should be respectively given in kilograms, years and centimeters. If not provided, default values will be used.

Some additional arguments can be used to change the streaming source:
```
  --camera_id=CAMERA_ID           ID of the camera to stream from
  --path_in=FILENAME              Video file to stream from. This assumes that the video was encoded at 16 fps.
```

It is also possible to save the display window to a video file using:
```
  --path_out=FILENAME             Video file to stream to
```

For the best performance, the following is recommended: 
- Place your camera on the floor, angled upwards with a small portion of the floor visible
- Ensure your body is fully visible (head-to-toe) 
- Try to be in a simple environment (with a clean background) 


#### Demo 3: Calorie Estimation

In order to estimate burned calories, we trained a neural net to convert activity features to the corresponding [MET value](https://en.wikipedia.org/wiki/Metabolic_equivalent_of_task).
We then post-process these MET values (see correction and aggregation steps performed [here](https://github.com/TwentyBN/sense/blob/master/sense/downstream_tasks/calorie_estimation/calorie_accumulator.py)) 
and convert them to calories using the user's weight.

If you're only interested in the calorie estimation part, you might want to use `scripts/calorie_estimation.py` which has a slightly more
detailed display (see video [here](https://drive.google.com/file/d/1VIAnFPm9JJAbxTMchTazUE3cRRgql6Z6/view?usp=sharing) which compares two videos produced by that script).

Usage:
```shell
PYTHONPATH=./ python scripts/run_calorie_estimation.py --weight=65 --age=30 --height=170 --gender=female
```

The estimated calorie estimates are roughly in the range produced by wearable devices, though they have not been verified in terms of accuracy. 
From our experiments, our estimates correlate well with the workout intensity (intense workouts burn more calories) so, regardless of the absolute accuracy, it should be fair to use this metric to compare one workout to another.

---

## Build your own classifier

This section will describe how you can build your own custom classifier on top of our models. Our models will serve
 as a powerful feature extractor that will reduce the amount of data you need to build your project. 

#### Step 1: Data preparation
Prepare the training and validation videos for each of the desired classes (labels) and organise them in this manner:

```
/path/to/your/dataset/
├── videos_train
│   ├── label1
│   │   ├── video1.mp4
│   │   ├── video2.mp4
│   │   └── ...
│   ├── label2
│   │   ├── video3.mp4
│   │   ├── video4.mp4
│   │   └── ...
│   └── ...
└── videos_valid
    ├── label1
    │   ├── video5.mp4
    │   ├── video6.mp4
    │   └── ...
    ├── label2
    │   ├── video7.mp4
    │   ├── video8.mp4
    │   └── ...
    └── ...
```
- Two top-level folders: one for the training data, one for the validation data.
- One sub-folder for each label with as many videos as you want (but at least one!)
- Requirement: videos should have a framerate of 16 fps or higher.

In some cases, as few as 2-5 videos per class have been enough to achieve excellent performance!

#### Step 2: Training

Once your data is prepared, run this command to train a customized classifier on top of one of our features extractor:
```shell
PYTHONPATH=./ python scripts/train_classifier.py --path_in=/path/to/your/dataset/ [--use_gpu] [--num_layers_to_finetune=9]
```

####  Step 3: Running your model

The training script should produce a checkpoint file called `classifier.checkpoint` at the root of the dataset folder.
You can now run it live using the following script:

```shell
PYTHONPATH=./ python scripts/run_custom_classifier.py --custom_classifier=/path/to/your/dataset/ [--use_gpu]
```

---

## iOS Deployment

If you're interested in mobile app development and want to run our models on iOS devices, please check out [sense-iOS](https://github.com/TwentyBN/sense-iOS) for step by step instructions on how to get our gesture demo to run on an iOS device.
One of the steps involves converting our Pytorch models to the CoreML format, which can be done from this repo using the following script:

```shell
python scripts/conversion/convert_to_coreml.py --backbone=efficientnet --classifier=efficient_net_gesture_control --output_name=sensenet
```

If you want to convert a custom classifier, set the classifier name to "custom_classifier", and provide the path to the dataset directory used to train the classifier using the "--path_in" argument.
```shell
python scripts/conversion/convert_to_coreml.py --backbone=efficientnet --classifier=custom_classifier --path_in=/path/to/your/dataset/ --output_name=sensenet
```

---

<!--
## Contributing
-->
<!-- Describe how developers can contribute --> 

## Citation

We now have a [blogpost](https://medium.com/twentybn/towards-situated-visual-ai-via-end-to-end-learning-on-video-clips-2832bd9d519f) you can cite:

```bibtex
@misc{sense2020blogpost,
    author = {Guillaume Berger and Antoine Mercier and Florian Letsch and Cornelius Boehm and 
              Sunny Panchal and Nahua Kang and Mark Todorovich and Ingo Bax and Roland Memisevic},
    title = {Towards situated visual AI via end-to-end learning on video clips},
    howpublished = {\url{https://medium.com/twentybn/towards-situated-visual-ai-via-end-to-end-learning-on-video-clips-2832bd9d519f}},
    note = {online; accessed 23 October 2020},
    year=2020,
}
```

---

## License 

The code is copyright (c) 2020 Twenty Billion Neurons GmbH under an MIT Licence. See the file LICENSE for details. Note that this license 
only covers the source code of this repo. Pretrained weights come with a separate license available [here](https://20bn.com/licensing/sdk/evaluation).
