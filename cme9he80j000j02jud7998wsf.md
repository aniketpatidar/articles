---
title: "What is GPT?"
datePublished: Wed Aug 13 2025 04:39:35 GMT+0000 (Coordinated Universal Time)
cuid: cme9he80j000j02jud7998wsf
slug: what-is-gpt
tags: chaiaurcode, chaicode

---

GPT = **Generative Pre-trained Transformer**  
  
A family of large language models that generate text one token at a time, using the Transformer architecture. GPTs are primarily used to generate text.

* **Generative** - it produces (generates) text.
    
* **Pre-trained** - first trained on huge amounts of text (next-token prediction).
    
* **Transformer** - the underlying architecture.
    

### How it works:

* Text is broken down into tokens by a tokenizer. Each token is turned into a vector.
    
* A series of Transformer decoder layers use self-attention and feed-forward networks to figure out the next-token distribution.
    
* During inference, the model either samples or chooses the next token with the highest probability, adds it, and repeats the process.
    

### Why GPT is powerful

* trained on massive text datasets.
    
* You can teach it new behavior by providing examples.