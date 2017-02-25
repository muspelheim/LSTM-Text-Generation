# Generating Text with an LSTM
![License](https://img.shields.io/github/license/philiparvidsson/Pymake2-Build-Tool.svg)

## What is this?
During the time that I was writing my bachelor's thesis *(Sequence-to-sequence Learning of Financial Time Series in Algorithmic Trading*, in which I used LSTM-based RNNs for modeling the problem), I became interested in natural language processing. After reading Andrej Karpathy's blog post titled [The Unreasonable Effectiveness of Recurrent Neural Networks](http://karpathy.github.io/2015/05/21/rnn-effectiveness/), I decided to give text generation using LSTMs for NLP a go. Although slightly trivial, the project still comprises an interesting program and demo, and gives really interesting (and sometimes very funny) results.

I implemented the program over the course of a weekend in [Hy](http://hylang.org) (a LISP built on top of Python) using Keras and TensorFlow. You can train the model on any text sources you like. Remember to give it enough time to go over at least fifty epochs, otherwise the generated text will not be very interesting, rather seemiling random garbage.

The LSTM is trained *character-by-character* (in contrast to *word-by-word*) which means that is learns to write *one single character* at a time to construct words. This has the peculiar effect of the LSTM making up words during generation, though still producing coherent sentences (with enough training)!

## Prerequisites
* [CUDA](http://nvidia.com/object/cuda_home_new.html)
* [cuDNN](https://developer.nvidia.com/cudnn)
* [h5py](http://h5py.org/)
* [Hy](http://hylang.org)
* [Keras](https://keras.io/)
* [NumPy](http://numpy.org)
* [Python](https://python.org)
* [TensorFlow](https://www.tensorflow.org/)

## Running the program
1. Install prerequisites:
   `pip install h5py hy keras numpy tensorflow-gpu`  
   <sub><i><b>NOTE:</b> Don't forget to install CUDA and cuDNN if you want to run the computations on your graphics card!</i></sub>  
2. Clone this repository:
   `git clone https://github.com/philiparvidsson/LSTM-Text-Generation`
3. Change working directory to it:
   `cd LSTM-Text-Generation`
4. Run it and specify sources:
   `./train.hy --sources lol.txt`

After each completed epoch, the program will save the model to a file. You can use it to generate text by invoking the program with different arguments:

`./train.hy --generate heylo`

Type `./train.hy --help` to see more information on how to use the program.

## Results

Below are a few interesting results attained by running the program on various corpora:

sdlgkdsg
