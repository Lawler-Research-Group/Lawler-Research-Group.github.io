---
layout: post
title: The Relativistic Quantum Information FAQ
date: 2022-10-30
description: by Eric Aspling
giscus_comments: true
tags: ["quantum", "information theory"]
---

A recent visit to Johns Hopkins University and the University of Maryland has me realizing that Relativistic Quantum Information (RQI) is too young in its infancy for many to know what it's all about. I received many questions during conversations with friends and colleagues about RQI. This FAQ is a collection of those questions and others that may prove helpful. Please feel free to add any questions to the comment section below, and I will do my best to answer them.

# Questions

- [What is RQI?](#question1)
- [What classifies as quantum information?](#question2)
- [What is relativistic about RQI?](#question3)
- [Why add relativity?](#question4)
- [When did RQI become an area of study?](#question5)
- [What is an Unruh-DeWitt detector?](#question6)
- [What other methods are used to study RQI?](#question7)
- [What are current RQI physicists studying?](#question8)
- [What future insights will RQI give?](#question9)

### What is RQI? <a name="question1"> </a>

RQI is a field of physics that combines the formalism of quantum information with relativity theory to understand how entanglement is transferred, created, and propagated in a relativistic setting.

### What classifies as quantum information? <a name="question2"></a>

Information is best thought of as the measurement of a state. The word **measurement** has an important role to play here as there are many different measures of individual states (e.g., temperature, velocity, spin, etc.). As a quick aside, a state is not necessarily quantum. It could be the state of a system or a state of being, all of which carry information. Nevertheless, this is how we describe information.

There are two forms of information; classical and quantum, but a defining feature that separates the two is **entanglement**. Quantum information is specific in that the measurement it's concerned with is entanglement. Of course, there are many measures of entanglement between quantum systems, but to be considered quantum information, it must contain entanglement. All other information classifies as classical. I recently wrote a [blog post](https://lawlergroup.lassp.cornell.edu/blog/2022/Quantum_channel/) on evaluating different types of information. If you are interested in a deeper dive, please check out that post and the references therein.

### What is relativistic about RQI? <a name="question3"></a>

The answer to this question is a bit tricky. The theory of RQI aims to tackle what we understand about quantum information, but in various ways that include relativity. To accomplish this, we use the language that handles quantum mechanics and special relativity; quantum field theory (QFT). However, QFT is a vast field. Specifically, the systems that I study, topological edge states of strongly correlated electrons, don't _necessarily_ include relativistic effects. Yet the machinery is utilized to great success and is equipped to handle relativistic velocities of Dirac fermions. Read on to find more (relativistic) use cases.

### Why add relativity? <a name="question4"></a>

Regardless of the motivation, this has repeatedly been the natural tendency of a physical theory. First, in the early 1900s, mechanics was revolutionized by Einstein, as well as others. Then later, in the 1930s and '40s, quantum mechanics was transformed into QFT by Dirac, Feynman, Schwinger, and more. This year (2022) brought the recognition of quantum information (QI) theory by Charles Bennet and Peter Shor et al. through various prizes, such as the 2023 Breakthrough Prize. Subsequently, the formalization of the theory in the mid to late 1990s wasn't the first attempt to understand QI but instead marked the birth of the field. One may surmise that the transformation to a theory of RQI was inevitable, and as such, we have gained many more insights.

### When did RQI become an area of study? <a name="question5"></a>

Similar to QI, RQI has been around for some time before the theory was laid out. It's hard to pin down when people began thinking about relativistic entanglement. But it was certainly no later than the work of [Alsing and Millburn](https://arxiv.org/abs/quant-ph/0302179)(2003) and formalized by [Eduardo Martin-Martinez](https://arxiv.org/abs/1106.0280)(2011). Shortly after, a boom in the field took place with the introduction of the [amps paradox](https://arxiv.org/abs/1207.3123)(2012) as well as the work developed by [Maldacena and Susskind](https://arxiv.org/abs/1306.0533)(2013) in their famous ER=EPR paper.

Eduardo's approach to formalizing the theory was to calculate quantum information values utilizing Unruh-DeWitt detectors as the machinery to transfer entanglement to quantum fields via interactions, encoding (decoding) QI onto (off of) quantum fields in a process known as entanglement harvesting. Subsequent years have led to progress in understanding the requirements to carry out this type of quantum channel, and [recent work](https://arxiv.org/abs/2210.12552) by John Marohn, Michael Lawler, and myself have connected this idea to experiment.

### What is an Unruh-DeWitt detector? <a name="question6"></a>

UDW detectors were proposed as a thought experiment in Bill Unruh's [1976 paper](https://journals.aps.org/prd/abstract/10.1103/PhysRevD.14.870) "Notes on Black-hole Evaporation". In this paper, Unruh imagined a detector with two states whose reading determined if radiation was present. These detectors, as he imagined, would be accelerated and subsequently detect radiation not present in an inertial frame of reference. This radiation became known as Unruh radiation.

Eventually, the machinery was found promising for entanglement harvesting, and today RQI physicists use UDWs to understand particle interactions in curved space-time. One [recent result](https://arxiv.org/abs/1908.07523) was the impracticality of wireless quantum communication by Simidzija et al. who showed that to decode the information from a quantum field, the detector would need to exist on the entirety of the light-cone that stems from the encoding interaction. This unreasonable requirement is what prevents wireless quantum communication.

### What other methods are used to study RQI? <a name="question7"></a>

As with QI, RQI has many subfields that most may not realize are studies of RQI. Two of the more famous problems tackled with RQI are ER=EPR and Emergent spacetime. ER=EPR links two famous papers of Einstein and, although controversial, ER=EPR tells a tale of entangled black holes linked together by a wormhole, which has consequences for the entangled connections of spacetime around the horizons of the black holes.

Recently I was introduced to studies on emergent spacetime. Take, for example, this 2016 [paper](https://arxiv.org/abs/1606.08444) by ChunJun Cao, Sean Carroll, and Spyridon Michalakis as well as the associated [blog post](https://www.preposterousuniverse.com/blog/2016/07/18/space-emerging-from-quantum-mechanics/). Carroll explains the result of this work as follows,

"_The claim, in its most dramatic-sounding form, is that gravity (spacetime curvature caused by energy/momentum) isn’t hard to obtain in quantum mechanics — it’s automatic! Or at least, the most natural thing to expect. If geometry is defined by entanglement and quantum information, then perturbing the state (e.g. by adding energy) naturally changes that geometry. And if the model matches onto an emergent field theory at large distances, the most natural relationship between energy and curvature is given by Einstein’s equation._"

As admitted by Carroll in the post, this work is controversial and speculative. However, these two areas, as well as others, are recent and exciting fields that utilize relativity and quantum information to shed light on the fundamental underpinnings of the universe.

### What are current RQI physicists studying? <a name="question8"></a>

The areas discussed in this FAQ are all topics modern RQI scientists are thinking about, but in case you skipped directly to this question, let's do a quick review with resources.

1.  Unruh-DeWitt detectors (This is a bit biased as this is my field of study)
    1a. UDW detectors used to understand wireless quantum communication; [Simidzija et al.](https://arxiv.org/abs/1908.07523)
    1b. UDW detectors to understand interactions in curved spacetime; [Tjoa et al.](https://arxiv.org/abs/2102.05734)
    1c. Unruh-DeWitt Quantum Computers which utilize the spin qubits coupled to Luttinger liquids to create flying qubits; [Aspling et al.](https://arxiv.org/abs/2210.12552)
2.  Understanding spacetime entanglement and connections to black holes via ER = EPR; [Maldacena and Susskind](https://arxiv.org/abs/1306.0533)
3.  Emergent space and time from entanglement; [Cao et al.](https://arxiv.org/abs/1606.08444)

There are many more topics in RQI, but this is a good starting place for the curious reader.

### What future insights will RQI give? <a name="question9"></a>

At this point, it's hard to tell what the future of RQI holds. Given that the field is very young, there is much work to do as we move forward. The insights we gain promise a deeper understanding of the nature of space and time but are also used practically to probe quantum information in quantum materials. A fully fleshed-out theory of relativistic quantum information will include these and everything in the middle. For that, there is much to look forward to.
