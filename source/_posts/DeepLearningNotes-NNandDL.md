---
title: DeepLearningNotes-NNandDL
date: 2018-10-22 12:32:25
tags: [Deep Learning, Coursera]
categories: [Deep Learning]

---

This is a note of the first course of the "Deep Learning Specialization" at [Coursera](https://www.coursera.org/specializations/deep-learning). The course is taught by Andrew Ng.

Almost all materials in this note come from courses' videos. The note combines knowledge from course and some of my understanding of these konwledge. Thus, it does not strictly follow the order of videos.

Some parts of this note are inspired from [Tess Ferrandez](https://www.slideshare.net/TessFerrandez/notes-from-coursera-deep-learning-courses-by-andrew-ng).

# Brief Intro to Deep Learning

To begin with, let's focus on some basic concepts to gain some intuition of deep learning.

## Stuctures of Deep Learning

We start with supervised learning. Here are several types of neural network (NN) in the folloing chart:

| INPUT: X | OUTPUT: y | NN TYPE |
| :------: | :------: | :------: |
| Home features | Price | Standard NN |
| Ad, user info | Click or not | Standard NN |
| Image | Objects | Convolutional NN (CNN) |
| Audio | Text Transcription | Recurrent NN (RNN) |
| English | Chineses | Recurrent NN (RNN) |
| Image, Radar info | Position of other cars | Custom NN |

Here are some pictures of Standard NN, Convolutional NN, Recurrent NN: 

<div align="center">
  <img src="Standard_NN.png" height="70%" width="70%" />
  <div class="image-caption">Standard NN</div>
  <img src="Convolutional_NN.png" height="70%" width="70%" />
  <div class="image-caption">Convolutional NN</div>
  <img src="Recurrent_NN.png" height="70%" width="70%" />
  <div class="image-caption">Recurrent NN</div>
</div>

## Data

Neural Nework can deal with both stuctured data and unstructured data. The following will give you an intuition of both kinds of data.

<div align="center">
  <img src="Structured_and_Unstructured_Data.png" height="50%" width="50%" />
  <div class="image-caption">Structured and Unstructured Data</div>
</div>

## Advantages

Here are some conclusions of why deep learning is advanced comparing to traditional machine learning.

Firstly, deep learning models performs better when dealing with big data. Here is a comparation of deep learning models and classic machine learing models:

<div align="center">
  <img src="Comparation_between_deep_learning_and_machine_learning.png" />
  <div class="image-caption">Comparation between deep learning and machine learning</div>
</div>

Secondly, thanks to the booming development of hardware and advanced algorithm, computing is much faster than before. Thus, we can implement our idea and know whether it works or not in short time. As a result, we can run the following circle much faster than we image.

<div align="center">
  <img src="Iteration_process.png" height="50%" width="50%" />
  <div class="image-caption">Iteration process</div>
</div>

# Basic Symbols of the course

These basic symbols will be used through out the whole specialization.

<div align="center">
  <img src="Standard_notations.png" />
  <div class="image-caption">Standard notations</div>
  <img src="Standard_representations.png" />
  <div class="image-caption">Standard representation</div>
</div>

Moreover, in this course, each input x will be stacked into columns and form the input matrix X.

<div align="center">
  <img src="Input_X.png" width="50%" height="50%" />
  <div class="image-caption">Input X</div>
  <img src="Output_y.png" width="50%" height="50%" />
  <div class="image-caption">Output y</div>
</div>

To be continue...
