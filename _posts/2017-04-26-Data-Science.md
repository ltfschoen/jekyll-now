---
layout: post
title: Tensor Flow (in progress!)
---

# Table of Contents
  * [Chapter 1 - Tensor Flow](#chapter-1)

## Chapter 1 - Tensor Flow <a id="chapter-1"></a>

* TensorFlow
    * Dfn: Separates the definition of computations from their execution
    even further by having them happen in separate places: a graph defines the operations,
    but the operations only happen within a session. Graphs and sessions are created
    independently. A graph is like a blueprint, and a session is like a construction site.
    Nesting lists is one way to represent a graph structure like a TensorFlow computation graph, i.e.
        ```
        $ foo.append(bar)
        $ foo
        [[...]]
        ```

* Links
    * Good Machine Learning Python example https://www.dataquest.io/blog/machine-learning-python/
    * http://www.bitfusion.io/2016/08/31/training-a-bird-classifier-with-tensorflow-and-tflearn/
    * Intro https://www.oreilly.com/learning/hello-tensorflow
    * Visualisations http://learningtensorflow.com/Visualisation/
    * Docker + Python + TensorFlow https://github.com/jupyter/docker-stacks/tree/master/tensorflow-notebook
        * http://stackoverflow.com/questions/33636925/how-do-i-start-tensorflow-docker-jupyter-notebook
    * Setup 101 http://www.datascienceassn.org/content/installation-quickstart-tensorflow-anaconda-jupyter
    * https://affinelayer.com/pixsrv/index.html
    * https://cloud.google.com/ml-engine/docs/how-tos/getting-started-training-prediction

* Data Sets (datasets)
    * https://archive.ics.uci.edu/ml/datasets.html
    * https://worldview.earthdata.nasa.gov/
    * https://earthexplorer.usgs.gov/