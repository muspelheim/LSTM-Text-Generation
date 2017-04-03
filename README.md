# Generating Text with an LSTM
![License](https://img.shields.io/github/license/philiparvidsson/LSTM-Text-Generation.svg)

## What is this?
During the time that I was writing my bachelor's thesis *[Sequence-to-Sequence Learning of Financial Time Series in Algorithmic Trading](https://github.com/philiparvidsson/Sequence-to-Sequence-Learning-of-Financial-Time-Series-in-Algorithmic-Trading)* (in which I used LSTM-based RNNs for modeling the thesis problem), I became interested in natural language processing. After reading Andrej Karpathy's blog post titled *[The Unreasonable Effectiveness of Recurrent Neural Networks](http://karpathy.github.io/2015/05/21/rnn-effectiveness/)*, I decided to give text generation using LSTMs for NLP a go. Although slightly trivial, the project still comprises an interesting program and demo, and gives really interesting (and sometimes very funny) results.

I implemented the program over the course of a weekend in [Hy](http://hylang.org) (a LISP built on top of Python) using Keras and TensorFlow. You can train the model on any text sources you like. Remember to give it enough time to go over at least fifty epochs, otherwise the generated text will not be very interesting, rather seemingly random garbage.

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
   <sup><i><b>&nbsp;&nbsp;&nbsp;&nbsp;NOTE:</b> If you want to perform computations on your graphics card, first install CUDA and cuDNN, then install `tensorflow-gpu` instead of `tensorflow` above.</i></sup>
2. Clone this repository:  
   `git clone https://github.com/philiparvidsson/LSTM-Text-Generation`
3. Change working directory to it:  
   `cd LSTM-Text-Generation`
4. Run the program and specify sources:  
   `./lstm.hy --sources corpus.txt`

After each completed epoch, the program will save the model to a file. You can then use it to generate text by invoking the program with the `--generate` switch and providing a seed text:

`./lstm.hy --generate "hello world"`

Type `./lstm.hy --help` to see more information on how to use the program.

### Example

If you have [Anaconda](https://www.continuum.io/), you can use the following sequence of commands to clone this repository, set up an environment, install dependencies and train the model on Shakespeare's texts:

```bash
git clone https://github.com/philiparvidsson/LSTM-Text-Generation
cd LSTM-Text-Generation
conda create -n lstm_shakespeare python=3 -y
source activate lstm_shakespeare
pip install h5py hy keras numpy tensorflow
mkdir data models
wget -O data/shakespeare.txt http://www.gutenberg.org/cache/epub/100/pg100.txt
./lstm.hy --batch-size 256 --layers 512,dropout:0.2,512 --learning-rate 0.001 --lookback 40 --model models/shakespeare --sources data/shakespeare.txt --stride 7
```

Let it run for at least fifty epochs (or until it reaches a *categorical accuracy* of at least 0.5—the higher the better), then hit ctrl-c to exit. Then, use the following command to generate text using the newly trained model:

```bash
./lstm.hy --generate "For thy sweet love remembered such wealth brings" --model models/shakespeare.txt
```

## Configuration
There are various settings to play with in the program. For the purpose of this program, there are no "optimal" settings- Rather, you should go ahead and experiment to come up with different, interesting results. If you want to see all available settings, type `./lstm.hy --help` in your terminal, with the working directory set to the path where the `lstm.hy` program is located.

### Batch size
The batch size is set like this: `--batch-size 256`. The default is 128.

### Disabling GPU acceleration
If you only want to do computations on the CPU (despite having installed GPU-enabled TensorFlow), specify the `--cpu` flag.

### Generation
Specifying the `--generate` command line argument and providing a seed text starts the program in *text generation mode* using the specified text as the initial seed for generation. You must train a model on a body of text before using the `--generate` command line argument. Example:

`--generate "hello world"`

### Configuring layers
The program defaults to a single 128-cell LSTM layer. You can specify custom layers using the `--layers` argument. For example, if we wanted to LSTM layers with 128 cells in the first and 64 in the last, with a dropout layer (with a dropout probability of 20%) inbetween, we would specify the following command line argument to the program:

`--layers lstm:128,dropout:0.2,lstm:64`  
<sup><i><b>&nbsp;&nbsp;&nbsp;&nbsp;NOTE:</b> The last layer must not be a dropout layer.</i></sup>

### Learning rate
The learning rate (which defaults to 0.01) can be set the following way:

`--learning-rate 0.001`

### Lookback
You can use the `--lookback` command line argument to specify the size (in number of characters) of the sliding window during training. The program defaults to a lookback value of 32 characters, but you can set it to anything you like (although a greater lookback value requires more memory). For example, if you want to take the last 50 charactesr into account during training, specify the following command line argument:

`--lookback 50`

### Model
Use the `--model` command line argument to specify the model name. This tells the program the name of the model to load or save. For example, to save to (or load from, if the `--generate` flag has been specified) a model named `my_model`:

`--model my_model`

### Stride
Using the `---stride` command line argument lets you set how many characters to move the sliding window forward after each training iteration. This setting can be thought of as a way to reduce the memory footprint for large corpora. The default is 3. Example:

`--stride 7`

### Word-by-word training *(not yet implemented)*
Specify the `--word-by-word` flag to train the model on one word at a time (i.e. training it to predict words) rather than one character at a time (i.e. training it to predict characters). Although this might result in more coherent generated sentences, this reduces the creative capability of the trained model—it will not be able to come up with new names or words.

## Troubleshooting

* **The model seems unable to learn**  
  The neural network model might have become unstable because of too high learning rate. You can try *lowering* the learning rate. For example, the default is 0.01, so you could try specifying 0.001: `--learning-rate 0.001`

## Results
Below are a few interesting results attained by running the program on various corpora.

**WARNING:** *There is some seriously offensive language ahead, please stop reading here if you are a sensitive person. The texts below have been generated by models trained on various corpora.*
___

### King James bible
I trained a model on the King James Bible for about a day on my Nvidia GTX 1080. The results were pretty funny and interesting:

— **9:7** *"And the man of vily made mad the land of Egypt."*

— **12:26** *"And God said now, Seruily, I will judge the mighty servants, five kindreds which the souls of thy bread people."*

— **18:34** *"For your feet, O Ishmel."*

— **119:6** *"The children of Israel said unto him, Fear upon the seven commandment, I will go us."*

### Snoop Dogg
Taking this a little further, I trained a model on Snoop Dogg song lyrics for a while and then let it generate new songs inspired by Snoop Dogg's work. Funny... and offensive!

**[Chorus: Amh Rock Jaz (Justin life]**  
*If you feel ya bitch, you know what it abuse?  
I'm all alone in the ride or you gon' change  
One of 'em smoke and be the motherfuckin' round  
Let me hear you get back to your  
Niggas gotta come with the big nickulaf  
So I'm gon' be? I make it lose it!  
Ahh, yeah, I'm the way you hit the pound of courser  
I can't be was that shit and I'm smokin' like O.I  
Gotta mather now!  
Niggas busta like a nigga in some real shit*

**[Hook]**  
*Now this is a sexual times  
But baby flip it, rollin around the party  
Cuz they know it make the whole world go bound  
And the rearte for my homies at this shit  
West dude, I'm a good hard, them boys  
See I'ma stuff that's the smoke  
With the wrong time, happens all the dirt  
Bounced in the black 18?3 or a trip  
Did you feel nothing butch chillin', and I'mma little back  
You know the clock where I problem, let's check  
Ya cuz just say, Yeah, we just light? Fuck me  
Delivery nigga*

### The King James Bible + Snoop Dogg
So why not combine two extremes into one? Here, I have let the program train a model on the King James Bible *and* Snoop Dogg song lyrics. The results are... well, judge for yourself. Offensiveness ahead.

— **16:21** *"Saying, The priest speak my soul in my motherfucker."*

— **22:13** *"And he shall not burn his weed for the stranger, when I can't call this shit was that taw it?"*

— **38:18** *"Ay you love out the pimpeth not die, which I cake bitches and a plocked before their sin"*

