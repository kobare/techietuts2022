---
layout: post
title:  "Text Summarization with LLMs: Techniques for Extractive and Abstractive Summarization"
author: Denis Kobare
date:   2024-12-01 09:30:00 +0300
img: /assets/img/svg/ai.svg
categories: more-topics
sub_category: machine-learning
type: neural-networks
technology: LLM
permalink: "category/:categories/machine-learning/neural-networks/:year:month/:title"
---


### Introduction

In today's information-rich world, the ability to quickly distill the essence of 
lengthy texts has become paramount. Text summarization, the process of 
condensing a piece of text while retaining its key information, has gained 
substantial attention due to its applications in news aggregation, content 
curation, and even aiding information accessibility for people with limited 
reading abilities. With the advent of Large Language Models (LLMs), such as 
GPT-3.5, text summarization has witnessed remarkable advancements. This article 
delves into the techniques and strategies for employing LLMs for both extractive 
and abstractive text summarization. We will compare their performance and 
provide practical implementation tips.



<br>
### Extractive Summarization

Extractive summarization involves selecting and assembling key sentences or 
phrases directly from the source text to form a summary. This technique is akin 
to "copy-pasting" sentences that encapsulate the main ideas. LLMs can assist in 
this process by ranking sentences based on their relevance and importance.


#### Implementation Steps:

1. Preprocessing: Start by tokenizing the input text into sentences or 
paragraphs, depending on the desired granularity of the summary. Remove any 
unnecessary formatting or special characters.

2. Embeddings: Utilize LLMs to convert each sentence into dense vector 
representations, often referred to as embeddings. These embeddings capture the 
semantic meaning of the sentences.

3. Sentence Scoring: Compute a score for each sentence based on its similarity 
to other sentences, length, and other features. Cosine similarity of embeddings 
is commonly used for this purpose.

4. Top Sentence Selection: Choose the top-scoring sentences to form the final 
summary. The challenge here is to strike a balance between diversity and 
relevance.

5. Post-processing: Reorder the selected sentences to enhance coherence and 
readability. Ensure that the summary flows naturally.

Here's a code snippet using Python and the transformers library for extractive 
summarization:

<br>
{% highlight python %}

from transformers import pipeline

# Load the pre-trained LLM model
summarizer = pipeline("summarization")

# Input text
input_text = "Your lengthy input text here..."

# Generate extractive summary
summary = summarizer(input_text, max_length=150, min_length=30, do_sample=False)[0]['summary_text']

print(summary)

{% endhighlight %}



<br>
### Abstractive Summarization

Abstractive summarization goes beyond extraction and involves generating concise 
and coherent summaries in a more human-like manner. LLMs are particularly 
powerful for abstractive summarization due to their creative text generation 
capabilities.


<br>
#### Implementation Steps:

1. Preprocessing: Similar to extractive summarization, tokenize the input text. 
Clean and preprocess the text, but this time, maintain more of the original 
structure.

2. Fine-tuning (Optional): For better control over the generated output, you 
might consider fine-tuning the LLM on a summarization dataset. This step 
requires substantial computational resources.

3. Text Generation: Use the fine-tuned or pre-trained LLM to generate summaries. 
Provide a prompt or a starting sentence that indicates the desired summary 
length and topic.

4. Beam Search: To enhance the quality of generated summaries, use beam search 
decoding. Beam search explores multiple possible continuations for the generated 
text and selects the most likely one.

5. Length and Quality Control: Apply length constraints and quality filters to 
the generated summary. You can achieve this by trimming excessively long 
summaries and reranking them based on coherence and informativeness.

Here's a code snippet for abstractive summarization using Python and the 
transformers library:

<br>
{% highlight python %}

from transformers import pipeline

# Load the pre-trained LLM model for summarization
summarizer = pipeline("summarization")

# Input text
input_text = "Your lengthy input text here..."

# Generate abstractive summary
summary = summarizer(input_text, max_length=150, min_length=30, do_sample=True)[0]['summary_text']

print(summary)

{% endhighlight %}



<br>
### Performance Comparison

In terms of performance, abstractive summarization often produces more coherent 
and fluent summaries, but it can sometimes generate inaccurate information. 
Extractive summarization, on the other hand, is more likely to preserve the 
original context but might lack fluency. The choice between the two techniques 
depends on the specific use case and desired trade-offs.



<br>
### Conclusion

Text summarization, facilitated by Large Language Models, has evolved to 
accommodate both extractive and abstractive techniques. While extractive 
summarization is relatively straightforward and can offer faithful 
representations of the source text, abstractive summarization allows for more 
creative and concise summaries. By following the implementation steps outlined 
in this article and utilizing the transformers library, developers and 
researchers can leverage the power of LLMs for text summarization across various 
applications. It's important to remember that fine-tuning and parameter tuning 
might be necessary to achieve optimal results, and careful evaluation is 
essential to ensure the quality of the generated summaries.



<br>
*Thanks for reading, see you in the next one!*
