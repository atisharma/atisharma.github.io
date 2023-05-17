---
layout: post
title: The inevitable proliferation of AI
categories: ai
---

[Alpaca] is a large language model released in March 2023, built upon on Facebook's [LLaMA] (released February 2023). Alpaca was trained by Stanford's HAI group, remarkably, for less than $500, using an adversarial approach ([self-instruction]) on output from OpenAI's text-davinci-003 (ChatGPT-3.5).

> There are two important challenges to training a high-quality instruction-following model under an academic budget: a strong pretrained language model and high-quality instruction-following data. The first challenge is addressed with the recent release of Meta’s new LLaMA models. For the second challenge, the self-instruct paper suggests using an existing strong language model to automatically generate instruction data. In particular, Alpaca is a language model fine-tuned using supervised learning from a LLaMA 7B model on 52K instruction-following demonstrations generated from OpenAI’s text-davinci-003.

Despite the relatively small size of the Alpaca model (7B parameters), it compares favourably to GPT-3.5 (175B) for many tasks. While the quality of a model's output depends on many factors including training data quantity and quality, model size, architecture and other things, it does appear that the data quantity and *quality* (in particular) are the constraining factor, despite the base model being trained on more or less the whole internet. Put another way, intelligence is training-data constrained and the training data information leaks through the output.

This apparent ease of acquiring intelligence from the output of a better model has consequences. The most important intelligence is "leaky"; once good models become accessible, open-source models will be trained quickly and cheaply to match the best available model at that time. Since inference is cheap compared to training, and incremental training using [low-rank adaptations] is itself cheap, intelligence will proliferate. Do not be surprised to find yourself having a discussion about supply-chain driven inflation with your fridge in the next 5-10 years.

Another consequence is that there is [no competitive moat] in AI models' intelligence. AI companies will have to differentiate along other lines, such as bespoke specialisation.

The final consequence is that it is impossible to regulate. Truly open-source innovation is happening in the open, on the internet, performed by individuals. Efforts by governments to constrain the capability of AI can only be applied to certain pressure points, which are large corporations. It is not practical, short of totalitarian tyrrany, to constrain the private acts of individuals to the extent that would be required. The same battle has been fought (and won) over [strong encryption].


[Alpaca]: https://crfm.stanford.edu/2023/03/13/alpaca.html
[LLaMA]: https://arxiv.org/abs/2302.13971v1
[self-instruction]: https://arxiv.org/abs/2212.10560
[low-rank adaptations]: https://arxiv.org/abs/2106.09685
[no competitive moat]: https://www.semianalysis.com/p/google-we-have-no-moat-and-neither
[strong encryption]: https://en.wikipedia.org/wiki/Pretty_Good_Privacy#History
