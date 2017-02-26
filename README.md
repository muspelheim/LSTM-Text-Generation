# Generating Text with an LSTM
![License](https://img.shields.io/github/license/philiparvidsson/Pymake2-Build-Tool.svg)

## What is this?
During the time that I was writing my bachelor's thesis *(Sequence-to-sequence Learning of Financial Time Series in Algorithmic Trading*, in which I used LSTM-based RNNs for modeling the thesis problem), I became interested in natural language processing. After reading Andrej Karpathy's blog post titled [The Unreasonable Effectiveness of Recurrent Neural Networks](http://karpathy.github.io/2015/05/21/rnn-effectiveness/), I decided to give text generation using LSTMs for NLP a go. Although slightly trivial, the project still comprises an interesting program and demo, and gives really interesting (and sometimes very funny) results.

I implemented the program over the course of a weekend in [Hy](http://hylang.org) (a LISP built on top of Python) using Keras and TensorFlow. You can train the model on any text sources you like. Remember to give it enough time to go over at least fifty epochs, otherwise the generated text will not be very interesting, rather seemiling random garbage.

The LSTM is trained *character-by-character* (in contrast to *word-by-word*) which means that it learns to write *one single character* at a time to construct words. This has the peculiar effect of the LSTM making up words during generation, though still producing coherent sentences (with enough training)!

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
   `pip install h5py hy keras numpy tensorflow`  
   <sup><i><b>NOTE:</b> If you want to perform computations on your graphics card, first install CUDA and cuDNN, then install `tensorflow-gpu` instead of `tensorflow` above.</i></sup>  
2. Clone this repository:
   `git clone https://github.com/philiparvidsson/LSTM-Text-Generation`
3. Change working directory to it:
   `cd LSTM-Text-Generation`
4. Run it and specify sources:
   `./lstm.hy --sources corpus.txt`

After each completed epoch, the program will save the model to a file. You can then use it to generate text by invoking the program with the `--generate` switch and providing a seed text:

`./lstm.hy --generate "hello world"`

Type `./lstm.hy --help` to see more information on how to use the program.

## Configuring your model

The program defaults to a single 128-cell LSTM layer. You can specify custom layers using the `--layers` argument. For example, if we wanted to LSTM layers with 128 cells in the first and 64 in the second, with a dropout layer (with a dropout probability of 20%) we would specify the following command line argument to the program:

`--layers lstm:128,dropout:0.2,lstm:64`  
<sup><i><b>NOTE:</b> The `lstm:` prefix is optional and may be omitted.</i></sup>

You may want to experiment with different layer configurations as they all give different results!

Furthermore, you can use the `--lookback` command line argument to specify the size (in number of characters) of the sliding window during training. The program defaults to a lookback value of 32 characters, but you can set it to anything you like (although a greater lookback value requires more memory). For example, if you want to take the last 50 charactesr into account during training, specify the following command line argument:

`--lookback 50`

Using the `---stride` command line argument lets you set how many characters to move the sliding window forward after each training iteration. This option can be thought of as a way to reduce the memory footprint for large corpora. The default is 3. Example:

`--stride 7`

## Results

Below are a few interesting results attained by running the program on various corpora:

sdlgkdsg
