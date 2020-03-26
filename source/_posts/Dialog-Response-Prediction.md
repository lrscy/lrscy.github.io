---
layout: post
title: Dialog Response Prediction Related Paper Read
date: 2019-02-11 20:40:58
tags: [NLP, DeepLearning]
categories: [NLP]
mathjax: true

---

Multi-turn Question-Answering/Chatbot problems are one of the hottest topics in NLP field recently. The blog sum up some tradition papers and several recent papers.

<!-- more -->

# [Multi-view Response Selection for Human-Computer Conversation](https://www.aclweb.org/anthology/D16-1036)

Tags: multi views, general attention, disagreement loss, likelihood loss

![Model Structure]('Multi-view_Response_Selection_for_Human-Computer_Conversation.png')

The structure of the model is clear. The essence of the model in my view is multi-view and multi-loss. By applying two levels (context level and sentence level) information extractor, the model can gather two independent view of the whole context. Also, two loss functions can be seen as two views on selecting answers. The only drawback of the model, in my view, is that the matching operation comes too late, just at the end of the model.

# [Sequential Matching Network A New Architecture for Multi-turn Response Selection in Retrieval-Based Chatbots (SMN)](https://arxiv.org/abs/1612.01627v2)

Tags: multi level info, dot self attention, CNN filter

![Model Structure](Sequential_Matching_Network_A_New_Architecture_for_Multi-turn.png)

The model's structure is basically clear. Just mention that the word pairs matrix comes from word embeddings rather than GRU which is the source of segment pair matrix. In the last layer, the author tried three methods for the L(.) function, the attention combination method works best, linear combination works worst, and last hidden state method works fair but fast. The author also use multi-view info to extract information from sentences, like what the previous paper did.
