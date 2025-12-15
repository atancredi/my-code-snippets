# Transformer’s Encoder-Decoder

[https://kikaben.com/transformers-encoder-decoder/](https://kikaben.com/transformers-encoder-decoder/)

The transformer architecture is the basis for recent well-known models like [BERT](https://arxiv.org/abs/1810.04805) and [GPT-3](https://openai.com/blog/gpt-3-apps/). Researchers have already applied the transformer architecture in [computer vision](https://arxiv.org/abs/2010.11929) and [reinforcement learning](https://arxiv.org/abs/2106.01345). The transformer is an encoder-decoder network at a high level, which is very easy to understand. The paper’s author says the architecture is simple because it has no recurrence and convolutions, so it’s neither a recurrent neural network nor a convolutional neural network.

![kb3hiib6.bmp](../../media/kb3hiib6.bmp)

## **Encoder-Decoder Architecture**

The original transformer published in the paper is a neural machine translation model.

![hxweqfo7.bmp](../../media/hxweqfo7.bmp)

The encoder extracts features from an input sentence, and the decoder uses the features to produce an output sentence (translation).

![b88e6k7t.bmp](../../media/b88e6k7t.bmp)

The encoder in the transformer consists of multiple encoder blocks. An input sentence goes through the encoder blocks, and the output of the last encoder block becomes the input features to the decoder.

![x90pk9rk.bmp](../../media/x90pk9rk.bmp)

The decoder also consists of multiple decoder blocks

Each decoder block receives the features from the encoder.

![rjtm5zip.bmp](../../media/rjtm5zip.bmp)

## **Input Embedding and Positional Encoding**

We tokenize an input sentence into distinct elements (tokens)

A tokenized sentence is a fixed-length sequence. For instance, if the maximum length is 200, every sentence will have 200 tokens with trailing paddings. These tokens are typically integer indices in a vocabulary dataset

To feed those tokens into the neural network, we convert each token into an embedding vector, a common practice in neural machine translation and other natural language models. In the paper, they use a 512-dimensional vector for such embedding. So, if the maximum length of a sentence is 200, the shape of every sentence will be (200, 512). The transformer learns those embeddings from scratch during training. 

Using the embedded lookup, the number of free parameters (matrix weights) only scales linearly with the number of words in the vocabulary. The same embedding vector is used for the same word regardless of where they are in a sentence. As such, we do not have the curse of dimensionality problem. ([source](https://kikaben.com/word-embedding-lookup/))

The point here is that an input to the transformer is not the characters of the input text but a sequence of embedding vectors. Each vector represents the semantics and position of a token. The encoder performs linear algebra operations on those vectors to extract the contexts for each token from the entire sentence and enrich the embedding vectors with helpful information for the target task through the multiple encoder blocks.

## Softmax and Output Probabilities

The decoder uses input features from the encoder to generate an output sentence. The input features are nothing but enriched embedding vectors.

The decoder outputs one token at a time. An output token becomes the 
subsequent input to the decoder. In other words, a previous output from 
the decoder becomes the last part of the next input to the decoder. This kind of processing is called “**auto-regressive**” - a typical pattern for generating sequential outputs and not specific to 
the transformer. It allows a model to generate an output sentence of different lengths than the input.