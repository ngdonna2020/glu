# GLU Variants Improve Transformer
Noam Shazeer (Google)

- [AI Genius?](https://www.wsj.com/tech/ai/noam-shazeer-google-ai-deal-d3605697?msockid=17cb7d10f5b46222298d6e23f49863b6)

train GLU on a samller sample for your code / claude artefact


## Context
The paper *GLU Variants Improve Transformer* addresses a key challenge in transformer models: improving the quality of the feed-forward layers. Traditionally, transformers have used ReLU (Rectified Linear Unit) or GELU (Gaussian Error Linear Unit) activations in their feed-forward sublayers. This paper explores variations of the Gated Linear Unit (GLU) as alternatives to these activations to enhance performance.

## Problem
Transformers often rely on ReLU or GELU activations in their feed-forward networks (FFNs). However, GLU (introduced in 2016), defined as the component-wise product of two linear transformations of the input (one of which is sigmoid-activated), offers a viable alternative. This paper proposes and tests several GLU variants to determine if they can improve upon the traditional ReLU or GELU activations.

<img width="300" alt="image" src="https://github.com/user-attachments/assets/b8d5fe9d-2a35-4d06-ba7c-1fc5a8b7e8ad">


*Question 1: What is representing gating mechanishm? Explain the idea of the "gate"?*

![image](https://github.com/user-attachments/assets/735c5b5d-26ed-4643-91e4-74c6e4e9a2a1)

Hint:
![6533a601586082158f57f44fa39ab3c6](https://github.com/user-attachments/assets/39e58b50-bff8-46c2-adc3-3c3d64b1d298)

The sigmoid function transforms those inputs whose values lie in the domain R, to outputs that lie on the interval (0, 1). 

GLU is a linear transformation followed by a gating mechanism. Here we see we have two trainable matrices W and V with V being used to calculate the gated unit. The gate provides an additional filter after the activation which can be learned during training and depends on the input itself. The ⊗ operation is the element-wise multiplication. xV+c acts as a filter for the other half of the operation. So depending on what the matrix values are in the filter, those same entries become prominent or are diminished from the sigmoid activation matrix.

## Approach
Shazeer and his team created multiple variations of GLU using different functions (like ReLU, GELU, or Swish) instead of sigmoid. They proposed additional variations on the Transformer FFN layer which use GLU or one of its variants in place of the first linear transformation and the activation function and omited the bias terms. To assess model quality, they calculated log-perplexity for comparison. Each fully-trained model was fine-tuned on a proportional mixture of the Stanford Question-Answering Dataset (SQuAD) and tasks from the GLUE and SuperGLUE benchmarks. The paper reports the best performance for each task based on the recorded checkpoints during fine-tuning.

$FFN_{RELU}(x, W_1, W_2)=RELU(xW_1)W_2$

<img width="317" alt="image" src="https://github.com/user-attachments/assets/3f644cfc-dbe1-48d6-81b2-77f23200c888">


*Question 2: All of the Transformer FFN layers which use GLU or one of its variants  have three weight matrices, as opposed to two for the original FFN. How do we keep the number of parameters and the amount of computation constant?*


## Pseudocode
See the Pseudocode file in the repo.

## How the Problem Was Addressed
The paper presents comparative results between different models in tabulated form. In a transfer-learning context, the new GLU variants show improved perplexities for the pre-training de-noising objective and perform better on many downstream language-understanding tasks. Specifically, the GEGLU and SwiGLU variants yielded the best perplexities. 


<img width="950" alt="image" src="https://github.com/user-attachments/assets/74769d83-1684-4141-bbf0-5873d68866b4">

Table 1: Heldout-set log-perplexity for Transformer models on the segment-filling task. All models are matched for parameters and computation.


<img width="911" alt="image" src="https://github.com/user-attachments/assets/140a21aa-a261-4dd1-92e0-0d8884b5ed97">

Table 2: GLUE Language-Understanding Benchmark


<img width="911" alt="image" src="https://github.com/user-attachments/assets/9e2f5104-723c-4713-9935-c8d69ba61ded">

Table 3: SuperGLUE Language-Understanding Benchmark


However, the author does not provide a detailed explanation for why these variants outperform the others, humorously attributing their success to "divine benevolence."

## Critical Analysis
While the paper demonstrates that GLU variants can enhance transformer performance, it lacks a deeper analysis of why these specific variants outperform ReLU and GELU. The success is attributed to "divine benevolence," leaving room for further research into the underlying mechanisms. Additionally, while the improvements are statistically significant, they are not drastic, raising the question of whether the added complexity is always warranted. An interesting divergence from standard practice is the lack of dropout during pre-training, which contributed to improved performance but could be explored further regarding its generalization effects.

## Impact
This paper contributes to the continuous evolution of transformer models, showing that even small adjustments like changing the activation function in the FFN layers can improve performance. These innovations could inform the development of more efficient and accurate models, especially for natural language processing (NLP) tasks. The findings intersect with previous efforts to refine transformers through alternative activation functions (e.g., Swish, GELU), suggesting further avenues for research into efficient architectures.

## Links
- [Original Paper: 2002.05202](https://arxiv.org/abs/2002.05202)
- [Base Model: 1910.10683](https://arxiv.org/abs/1910.10683)
- [GELU: 1606.08415](https://arxiv.org/abs/1606.08415)
- [GLUE Benchmark: 1804.07461](https://arxiv.org/abs/1804.07461)
- [SuperGLUE Benchmark: 1905.00537](https://arxiv.org/abs/1905.00537)
- [Noam Shazeer - Google Deal on WSJ](https://www.wsj.com/tech/ai/noam-shazeer-google-ai-deal-d3605697?msockid=17cb7d10f5b46222298d6e23f49863b6)

## Citation
Shazeer, N. (2020). *GLU Variants Improve Transformer*. [arXiv:2002.05202](https://arxiv.org/abs/2002.05202).

Raffel, C., Shazeer, N., Roberts, A., Lee, K., Narang, S., Matena, M., Zhou, Y., Li, W., & Liu, P. (2019). *Exploring the limits of transfer learning with a unified text-to-text transformer*. [arXiv:1910.10683](https://arxiv.org/abs/1910.10683).

Wang, A., Singh, A., Michael, J., Hill, F., Levy, O., & Bowman, S. R. (2018). *GLUE: A multi-task benchmark and analysis platform for natural language understanding*. [arXiv:1804.07461](https://arxiv.org/abs/1804.07461).

Wang, A., Pruksachatkun, Y., Nangia, N., Singh, A., Michael, J., Hill, F., Levy, O., & Bowman, S. R. (2019). *SuperGLUE: A stickier benchmark for general-purpose language understanding systems*. [arXiv:1905.00537](https://arxiv.org/abs/1905.00537).
