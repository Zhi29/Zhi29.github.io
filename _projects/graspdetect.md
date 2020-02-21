---
title: 'Grasping Detection'
subtitle: 'Teach robot how to grasp an object'
date: 2018-06-30 00:00:00
featured_image: '/images/projects/refpaper.jpg'
---

## Overview 

The goal of this project is to infer grasping location given an object for robotic grippers by using deep learning algorithms. ResNet based neural networks is built and implemented by using dataset from Cornell robot learning group. We also implement simple Variational Autoencoder for data augmentation. We compared the performance on architecture based on ResNet18 and ResNet34 with augmented data. 

## Introduction 

Robotic grasping is a challenging problem for many years. One research direction for these problems are to convert the perception aspect into a detection problem where, given noisy, partial view of the object from a camera, the goal is to infer the top locations where a robotic gripper could be placed. The goal for this project is not only to infer a viable grasp, but to infer the optimal grasp for a given object that maximizes the chance of successfully grasping it.

## Method

We use transfer learning method to load famous Deep CNN ResNet as our main architecture and make pixel information as our input. Also we grab the mainly part of ResNet and change the fully-connected layers. Thus the output could be 1 possible grasping boxes coordinates.

## Dataset 

our [dataset](http://pr.cs.cornell.edu/grasping/rect_data/data.php) is provided by Robot Learning Lab at Cornell University and [Jacquard dataset](https://jacquard.liris.cnrs.fr/).

