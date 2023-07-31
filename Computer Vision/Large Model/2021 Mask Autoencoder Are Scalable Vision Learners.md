# Mask Autoencoder Are Scalable Vision Learners

MAE 最突出的贡献就是提出了预训练视觉大模型。

## 0 Abstract

We mask random patches of the input image and reconstruct the missing pixels.

First, we develop an asymmetric encoder-decoder architecture, with an encoder that operates only on the visible subset of patches (without mask tokens), along with a lightweight decoder that reconstructs the original image from the latent representation and mask tokens.

Second, we find that masking a high proportion of the input image, e.g., 75%, yields a nontrivial and meaningful self-supervisory task.

Our scalable approach allows for learning high-capacity models that generalize well: e.g., a vanilla ViT-Huge model achieves the best accuracy (87.8%) among methods that use only ImageNet-1K data.

Transfer performance in downstream tasks outperforms supervised pre-training and shows promising scaling behavior.

## 1 Introduction

The solutions, based on autoregressive language modeling in GPT and masked autoencoding in BERT, are conceptually simple: they remove a portion of the data and learn to predict the removed content. These methods now enable training of generalizable NLP models containing over one hundred billion parameters.

What makes masked autoencoding different between vision and language?

1. Architectures were different: Convolutions typically operate on regular grids and it is not straightforward to integrate ‘indicators’ such as mask tokens or positional embeddings into convolutional networks. This architectural gap, however, has been addressed with the introduction of Vision Transformers (ViT) and should no longer present an obstacle.
2. Information density: Languages are human-generated signals that are highly semantic and information-dense. Images, on the contrary, are natural signals with heavy spatial redundancy.
3. Decoder: In vision, the decoder reconstructs pixels, hence its output is of a lower semantic level than common recognition tasks. This is in contrast to language, where the decoder predicts missing words that contain rich semantic information. While in BERT the decoder can be trivial (an MLP), we found that for images, the decoder design plays a key role in determining the semantic level of the learned latent representations.

Our MAE masks random patches from the input image and reconstructs the missing patches in the pixel space. It has an asymmetric encoder-decoder design. Our encoder operates only on the visible subset of patches (without mask tokens), and our decoder is lightweight and reconstructs the input from the latent representation along with mask tokens. Shifting the mask tokens to the small decoder in our asymmetric encoder-decoder results in a large reduction in computation. 

## 2 Related Work

## 3 Approach

An encoder that maps the observed signal to a latent representation, and a decoder that reconstructs the original signal from the latent representation.

### Masking

Following ViT, we divide an image into regular non-overlapping patches. We sample random patches without replacement, following a uniform distribution.

### MAE encoder

Our encoder is a ViT but applied only on visible, unmasked patches. Just as in a standard ViT, our encoder embeds patches by a linear projection with added positional embeddings, and then processes the resulting set via a series of Transformer blocks.

### MAE decoder

The input to the MAE decoder is the full set of tokens consisting of (i) encoded visible patches, and (ii) mask tokens. Each mask token is a shared, learned vector that indicates the presence of a missing patch to be predicted. We add positional embeddings to all tokens in this full set; without this, mask tokens would have no information about their location in the image. The decoder has another series of Transformer blocks.

The MAE decoder is only used during pre-training to perform the image reconstruction task (only the encoder is used to produce image representations for recognition). Therefore, the decoder architecture can be flexibly designed in a manner that is independent of the encoder design. We experiment with very small decoders, narrower and shallower than the encoder.

### Reconstruction target

Predicting the pixel values for each masked patch: Each element in the decoder’s output is a vector of pixel values representing a patch. The last layer of the decoder is a linear projection whose number of output channels equals the number of pixel values in a patch.

Our loss function computes the mean squared error (MSE) between the reconstructed and original images in the pixel space. We compute the loss only on masked patches. This choice is purely result-driven: computing the loss on all pixels leads to a slight decrease in accuracy (e.g., ∼0.5%).

We also study a variant whose reconstruction target is the normalized pixel values of each masked patch. Specifically, we compute the mean and standard deviation of all pixels in a patch and use them to normalize this patch. Using normalized pixels as the reconstruction target improves representation quality in our experiments.

### Simple implementation

First we generate a token for every input patch (by linear projection with an added positional embedding). Next we randomly shuffle the list of tokens and remove the last portion of the list, based on the masking ratio. After encoding, we append a list of mask tokens to the list of encoded patches, and unshuffle this full list (inverting the random shuffle operation) to align all tokens with their targets. The decoder is applied to this full list (with positional embeddings added).

## 4 ImageNet Experiments

### Baseline: ViT-Large

We note that it is nontrivial to train supervised ViT-L from scratch and a good recipe with strong regularization is needed (82.5%). Even so, our MAE pre-training contributes a big improvement. Here fine-tuning is only for 50 epochs (vs. 200 from scratch), implying that the fine-tuning accuracy heavily depends on pre-training.

### 4.1 Main Properties

The ratio of 75% is good for both linear probing and fine-tuning. This behavior is in contrast with BERT, whose typical masking ratio is 15%. Our masking ratios are also much higher than those in related works in computer vision (20% to 50%).

The model infers missing patches to produce different, yet plausible, outputs. It makes sense of the gestalt of objects and scenes, which cannot be simply completed by extending lines or textures.

For linear probing, the accuracy increases steadily with the masking ratio until the sweet point: the accuracy gap is up to ~20% (54.6% vs. 73.5%). For fine-tuning, the results are less sensitive to the ratios, and a wide range of masking ratios (40-80%) work well. All fine-tuning results in Figure 5 are better than training from scratch (82.5%).

### Decoder design

A sufficiently deep decoder is important for linear probing. This can be explained by the gap between a pixel reconstruction task and a recognition task: the last several layers in an autoencoder are more specialized for reconstruction, but are less relevant for recognition. A reasonably deep decoder can account for the reconstruction specialization, leaving the latent representations at a more abstract level. This design can yield up to 8% improvement in linear probing.

If fine-tuning is used, the last layers of the encoder can be tuned to adapt to the recognition task. The decoder depth is less influential for improving fine-tuning.

Interestingly, our MAE with a single-block decoder can perform strongly with fine-tuning (84.8%). Note that a single Transformer block is the minimal requirement to propagate information from visible tokens to mask tokens. Such a small decoder can further speed up training.

| decoder depth |    ft    |   lin    |
| :-----------: | :------: | :------: |
|       1       |   84.8   |   65.5   |
|       2       |   84.9   |   70.0   |
|       4       |   84.9   |   71.9   |
|       8       | **84.9** | **73.5** |
|      12       |   84.4   |   73.3   |

