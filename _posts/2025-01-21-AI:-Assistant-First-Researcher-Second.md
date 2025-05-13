---
layout: post
title: "AI: Assistant First, Researcher Second"
date: 2025-1-21 11:31:50
description: by Tristan Galler
comments: true
tags: neuralnets, materials-science, RAG
---

Four months ago, I embarked on an ambitious experiment: I wanted to prompt-engineer ChatGPT-3 into constructing a variance matrix from data of multiple Ising models, ultimately allowing it to generate its own Ising model based on the collected data. This task, which seemed plausible given the growing capabilities of AI, quickly illuminated the limitations of large language models (LLMs) in high-level scientific reasoning and computation. Despite its fluency and broad knowledge base, GPT-3 struggled with rigorous, mathematically intensive tasks that required deep reasoning and autonomous problem-solving. This realization, coupled with discussions with my mentor, spurred an ongoing inquiry: To what extent can foundational LLMs (large dataset AI such as ChatGPT or Llama) be fine-tuned for scientific research, particularly in condensed matter theory?

### The Challenge of Fine-Tuning LLMs for Science

While fine-tuning LLMs for domain-specific expertise is a promising approach, the availability of well-trained scientific models remains limited. The closest counterpart I found was [Xiwu](https://arxiv.org/abs/2404.08001) [1], a recently developed L2 (fine-tuned from a foundational model) LLM for high-energy physics. However, the landscape for condensed matter physics was sparse, and much of our data—such as scanning tunneling microscopy (STM) images—was more efficiently stored as images rather than text. Given these constraints, my mentor and I decided to pivot towards evaluating multimodal LLMs (MLLMs), particularly image-based models, to determine their efficacy in scientific analysis.

### The Role of Multimodal LLMs

I have experience running LLMs locally, primarily using Llama models, which have evolved significantly from Llama2 to the contemporary Llama3.3. Historically, deploying and fine-tuning these models locally has been a cumbersome process, requiring different installation methods and optimization strategies depending on the specific model. However, the release of [Ollama](https://ollama.com/)—a streamlined platform for running LLMs and MLLMs—greatly simplified this workflow. In the past two months, Ollama has expanded to support a diverse range of MLLMs, while also providing tools for UI development, API integration, and parameter management.

To evaluate the capabilities of these MLLMs, I ran five different models using raw STM data from various materials, such as graphene. The largest model I tested, Llama3.2 Vision (11 billion-parameter model), demonstrated an impressive ability to recognize the general context of the images, even making reasonable predictions about the material composition and possible defects. However, all other models exhibited severe hallucinations, misidentifying the STM images as blankets or cloths.

<p align="center">
<img src="https://i.imgur.com/Y8KMmkY.jpeg" width="500" />
</p>
<p align="center">
    <b>Fig. 1 An example of STM data sent to the LLMs, the material here being graphite [2].</b>
    </p>

### The Impact of Prompt Engineering

In an attempt to first refine the models’ performance without adding new data, I implemented a simple yet effective intervention: prompt engineering. By adding a subtext to each prompt (essentially instructing the models to assume the role of a distinguished professor in condensed matter physics) I observed a dramatic improvement in their responses. Now, all five models correctly identified the STM technique and provided coherent analyses of material defects. However, this approach had a crucial limitation: it did not enhance the models' ability to generate figures or perform rigorous quantitative analysis. Rather, it merely refocused their responses toward condensed matter physics without deepening their computational reasoning.

<p align="center">
<img src="https://i.imgur.com/qh0kYN3.png" width="80%"/>
</p>
<p align="center">
<img src="https://i.imgur.com/ABVdcdy.png" width="80%"/>
</p>
<p align="center">
    <b>Figs. 2,3 The prompt engineered modification of Llava, a moderately large model, answering without hallucination.</b>
    </p>

### Exploring Retrieval-Augmented Generation

Recognizing that additional data might be required, I turned to Retrieval-Augmented Generation (RAG), a technique that embeds external data into the model’s vector space, allowing it to better match prompts with relevant information. However, even when embedding the papers that contained the images I uploaded, there was no improvement in recognition or scientific reasoning. In fact, without prompt engineering, the smaller models continued to hallucinate no matter the parameter of attention I put on the papers within the LLM.

### GPT-4: The Standout in Multimodal Reasoning

Despite my focus on open-source solutions, I have also been experimenting with GPT-4, which supports multimodal capabilities. Unlike the other models I tested, GPT-4 has shown remarkable proficiency in analyzing images, even providing annotated feedback on STM data. It remains the only model capable of returning modified images, highlighting possible defects for example, rather than just describing them in text. Furthermore, GPT-4 has demonstrated greater flexibility in fine-tuning, offering a glimpse into the potential of well-optimized multimodal AI for scientific research.

<p align="center">
<img src="https://i.imgur.com/FPioA8f.png" width="500"/>
</p>
<p align="center">
    <b>Fig. 4 GPT-4's impressive ability to communicate ideas through modifying uploaded images, evidenced by the red circles indicating possible defects.</b>
    </p>

### The Future of AI in Scientific Research

These findings reinforce a key takeaway: while AI excels as a personalized assistant—summarizing papers, structuring research plans, and even contextualizing complex scientific ideas—it still falls short as an autonomous scientific researcher. The ability to engage in high-level reasoning, generate rigorous mathematical models, and independently conduct research on a local level remains beyond the reach of current LLMs and MLLMs, even with fine-tuning and retrieval techniques.

However, the rapid evolution of AI suggests that these gaps may narrow in the coming years. With better fine-tuning techniques, accessible multimodal embedding programs, and more robust open-source research models, AI could eventually transition from a highly capable assistant to a legitimate collaborator in scientific discovery. Until then, researchers must leverage AI for what it currently does best—enhancing efficiency, improving accessibility, and accelerating preliminary analysis—while acknowledging its limitations in deep scientific reasoning.

In the meantime, my next steps involve further experiments with RAG implementations, exploring alternative fine-tuning methods, and closely monitoring advancements in MLLMs. Whether through open-source models or proprietary solutions like GPT-4, the potential of AI in condensed matter physics remains an exciting frontier, albeit one that still requires significant human expertise to navigate effectively.

[1]Zhang, Zhengde, et al. "Xiwu: A Basis Flexible and Learnable LLM for High Energy Physics", arXiv:2404.08001, arXiv, 8 Apr. 2024. arXiv.org, https://doi.org/10.48550/arXiv.2404.08001.

[2]“Capillary Force-Induced Superlattice Variation atop a Nanometer-Wide Graphene Flake and Its Moiré Origin Studied by STM.” Beilstein Journal of Nanotechnology, vol. 10, Jan. 2019, pp. 804–10. www.sciencedirect.com, https://doi.org/10.3762/bjnano.10.80.
