---
title: "AI in Drug Discovery: Data Quality, Not Quantity, Is the Bottleneck"
description: "Two important essays landed in August 2026 on AI in drug discovery: Bender et al. in Nature Reviews Drug Discovery, and Daphne Koller on a16z. Both are worth reading. Here is where I push back."
author: Ming Tang
date: '2026-08-17'
slug: ai-drug-discovery-data-quality-not-quantity
categories:
  - bioinformatics
  - genomics
tags:
  - AI
  - drug-discovery
  - single-cell
  - data-quality
  - foundation-models
  - biotech
  - bioinformatics
math: false
header:
  caption: ''
  image: ''
---

Two important essays on AI in drug discovery landed in August. Both are worth your time. Both have the right intuitions. And one of them is subtly framing the wrong problem, in my opinion.

The first is the [Nature Reviews Drug Discovery review](https://www.nature.com/articles/s41573-026-01496-2) by Andreas Bender, Morgan Thomas, Jack Scannell, David Shaywitz, Gian Marco Ghiandoni, Joe Greener, and colleagues: "Artificial intelligence in drug discovery — what it is, where we stand and the path forward." It is a sober, structured critique of the field. The single line that stuck with me: "evidence of their clinically relevant impact is, so far, disappointingly limited." That is Nature Reviews Drug Discovery saying it, not a random skeptic on X.

![Nature Reviews Drug Discovery article on AI in drug discovery](/img/nature_review.png)

*From Bender et al., [Nature Reviews Drug Discovery, August 2026](https://www.nature.com/articles/s41573-026-01496-2).*

The second is Daphne Koller's [a16z essay "Drug Discovery Has No Magic Wands"](https://www.a16z.news/p/drug-discovery-has-no-magic-wands). Koller is the founder and CEO of insitro and one of the most thoughtful people in the AI-for-biology space. Her core claim: the bottleneck in drug discovery is not molecular design. The bottleneck is disease understanding, choosing the right mechanism. She debunks four AI "magic wand" narratives, notes that most AI investment sits in stage 2 (molecule design) even though stages 1 and 3 dominate the failure rate, and calls out that AI has not yet delivered a single case of drugging the undruggable.

I agree with almost all of it. But there is one specific claim in Koller's piece where I want to push back, because the framing has downstream consequences for how biotech spends its research dollars.

## Where the two essays converge

Both papers make the same core observations, from different angles.

**AI has not moved the clinical needle yet.** The Nature Reviews paper says it in the abstract. Koller says the industry advanced roughly 100 novel targets per year in 2015 and only around 30 in 2024. The molecular design tools have gotten better. Approvals have not scaled. If the input-output relationship were tight, you would expect approvals to go up. They have not.

**The problem is not molecule design.** Ninety percent of clinical trials fail, mostly because we picked the wrong mechanism, not because the molecule was poorly optimized. RFdiffusion can generate a picomolar binder in a weekend. That does not help you if the target is not causally driving the disease.

**Human biology is the constraint.** Rodents do not get Alzheimer's. Non-human primates do not recapitulate ALS. You can generate all the mouse data you want and still not know what will happen in a patient. This is the core reason I keep saying the ultimate test is human, not silico.

**Better clinical trials are downstream of better biology.** Koller's phrasing is spot on. You cannot AI-accelerate your way out of a trial that was designed around the wrong hypothesis. Faster failure is not the same as fewer failures.

I have made most of these arguments in [my own post on where AI drug discovery actually stands in 2026](/post/has-ai-changed-drug-development-3-years-later/). The convergence is not surprising. Anyone paying attention arrives at the same conclusions.

## What the Nature Reviews paper adds

The Bender et al. review is worth reading in full because it adds a structural critique that Koller does not fully develop. Three points from the paper I found sharpest:

- **Technology push vs science pull.** Most of the AI drug discovery ecosystem is "we built a model, what problem can we point it at?" The direction of pull should be reversed. The biologists have a question. The right computational method should be selected or built to answer it. Anyone who has sat in a startup pitch where a founder demos an LLM and then hunts for a therapeutic area to attach it to will recognize this pattern.

  The best framing I heard on this came from Anthony Philippakis at Google Ventures, on a panel I co-hosted with Shirley Liu at the [ISMB 2026 industry workshop](https://ismb-2026-industry-workshop.vercel.app/). His line: "You don't grab a hammer and then go hunting for nails. You start with the nail. The problem. Then you decide what drives it in. Maybe a hammer. Maybe a brick already does the job." I wrote up my take in a [LinkedIn post](https://www.linkedin.com/feed/update/urn:li:share:7491489595222667264/) right after. The question is not "how do we use AI here." It is "where is the real value, and what actually speeds it up." Sometimes the honest answer is that AI is not the tool. Sometimes the brick already does the job.

- **Underspecified problem definitions.** Models trained to hit a benchmark score do not solve real-world drug discovery decisions. The benchmark is not the job. Pat Walters made this point in his ["Please Stop Fishing in the Bathtub"](https://patwalters.github.io/Please-Stop-Fishing/) post: benchmark wins on in-distribution data tell you nothing about the messy, program-specific decisions medicinal chemists actually make.

- **Rebenchmarking on decision quality, not model quality.** The paper's punchline recommendation is that we should evaluate whether AI actually improves decisions, not whether the model beats a baseline on a curated test set. This is a small phrasing shift with big consequences. It moves the field away from Kaggle-style leaderboard competition and toward whether the drug program lands a viable candidate faster.

None of this is new to anyone who has spent time in a pharma bioinformatics team. But it is powerful to see it argued cleanly in Nature Reviews Drug Discovery.

## Where I push back on Koller

Here is the specific claim I want to argue with. Koller writes that "the largest cell atlases remain orders of magnitude too small to cover this space." Her prescription is more data at larger scale, with an emphasis on covering more of the biological state space experimentally.

![Data amount needed for AI drug discovery, from Koller's a16z essay](/img/data_amount.png)

*Figure from Daphne Koller's [a16z essay](https://www.a16z.news/p/drug-discovery-has-no-magic-wands), showing the estimated data volume needed to model human biology at scale.*

I do not think more data at current quality solves the problem. I think the problem is data quality, not data quantity. Multiply GEO by 100x and you still have the same issues. Bigger is not better if bigger inherits the same failure modes.

Here is why I hold that view.

**GEO already has enormous scale, and the quality problem dominates.** My friend Jinfeng Zhang once analyzed all of the Gene Expression Omnibus and found sample mislabeling as high as 25% in some contexts. A lab's "MCF7 breast cancer" cells may actually be HeLa. Cell line contamination and misidentification is not exotic; it is documented across the field. The scale is not the bottleneck. The label quality is. Training a foundation model on 25%-mislabeled data does not give you biology. It gives you 25% garbage plus a hallucination engine.

**Foundation models trained on massive scRNA-seq atlases have not beaten linear baselines.** The [Kedzierska et al. paper in Nature Methods 2025](https://www.nature.com/articles/s41592-025-02772-6) tested five single-cell foundation models against linear baselines on perturbation prediction. The deep learning models did not outperform the linear baselines. The atlases involved were not small. Anshul Kundaje was even blunter in his [March 2026 thread](https://x.com/anshulkundaje/status/2034317728878907546): the "virtual cell models" trained on scRNA-seq atlases are basically doing fancy nearest neighbor search, and a literal nearest neighbor lookup with prior knowledge embeddings often outperforms the giant pretrained models. If the scale-heavy approach were working, we would see it in the benchmarks. We do not.

**The biology is stored dynamically, but we measure it statically.** Almost every sequencing readout is a snapshot of dead cells. The cells were alive, dynamic, exchanging signals with neighbors. Then we dissociated them, lysed them, and read out their final state. The biology that mattered, the dynamics that produced the state, is gone by the time we have the count matrix. I wrote about this on [LinkedIn earlier this year](https://www.linkedin.com/posts/%F0%9F%8E%AF-ming-tommy-tang-40650014_unlocking-the-true-potential-of-ai-and-activity-7448727853799497729-ixK1). More snapshots of dead cells is not the answer. Real-time, live-cell, dynamics-aware readouts are the answer, and we barely have those at any scale.

**Even single-cell RNA is one layer of the truth.** Take PI3K/AKT signaling.

![PI3K/AKT/mTOR pathway diagram](/img/PI3K.jpeg)

*Figure adapted from [He et al., Signal Transduction and Targeted Therapy 6, 425 (2021)](https://www.nature.com/articles/s41392-021-00828-5).*

Seven receptor types feeding in, three PI3K classes, two mTOR complexes branching out, and most of the regulation happening through phosphorylation and dephosphorylation events that RNA-seq cannot see at all. AKT alone has been reported to phosphorylate over a hundred different substrates. TRAF6 controls AKT through ubiquitination, a completely different chemical mark. A gene can be expressed at a normal level and still be functionally silent because the kinase that was supposed to activate it never fired. RNA gives you one snapshot of one layer. No amount of extra scRNA-seq atlas coverage recovers the proteomics, phospho-state, trafficking, and turnover data that are not in the count matrix. I wrote a longer version of this argument on [LinkedIn recently](https://www.linkedin.com/feed/update/urn:li:share:7492576757078781952/): data is the bottleneck, not your foundation models. No architecture fixes a missing dimension of data.

**Batch effects and processing artifacts scale with the data.** Even data from the same lab, same cell line, different day, will show meaningful batch effects. Multiply by 100 labs and 10 protocols and 3 platforms and you now have a batch-effect problem that no normalization method fully solves. Single-cell RNA-seq has the additional joy of heat-shock signatures from dissociation, dropout, and transcriptional drift. More data adds more variance sources unless quality control scales with volume, and it usually does not.

**Metadata is where the actual scaling problem lives.** In [my post on biotech data strategy](/post/biotech-data/) I argued that data without metadata is like a library without labels. That framing was not rhetorical. It is the literal experience of trying to build models on top of public repositories where sample condition is a free-text field and half the time it is missing. You cannot condition on a variable that was never recorded. You cannot even filter to the right patient subgroup if the collection date, tissue site, treatment history, and prior therapy fields are inconsistent across studies.

The pattern here is not "we need more data." It is "we need data that was collected with the specific question in mind, with clean labels, matched contexts, dynamics preserved where possible, and metadata that lets a model condition on the biology that matters."

Koller knows this. Her essay is more nuanced than "more data" and I want to be fair to her framing. She emphasizes "measuring biology correctly" and "targeted causal models through strategic experimentation." But the headline framing of "orders of magnitude too small" invites readers to think the fix is more scale, and I think that framing is wrong for anyone allocating capital in this space right now. The fix is more relevant data, not more data.

## Where "quality" actually breaks down in practice

If quality is the bottleneck, what does that mean concretely? Five things, in my experience:

**Right context.** A cell atlas of healthy tissue is not the right training set for a disease model. A HEK293 perturbation atlas is not the right training set for T-cell biology in a tumor microenvironment. The "universal foundation model on all of biology" pitch keeps failing because biology is context-dependent, and the context has to match the use case. Anshul Kundaje said it well: you can absolutely learn cis-regulatory logic from cell-line data, but only if you match the training context to the inference context. The pipe dream is training on arbitrary contexts and hoping the model generalizes to arbitrary other contexts. The amount of data that would take is astronomical.

**Right modality.** Bulk RNA-seq averages out cell type composition and hides the biology you care about. Single-cell adds resolution but destroys spatial context. Spatial transcriptomics adds spatial context but loses depth. Every modality throws information away. The right question is: what modality captures the biology that drives my endpoint? For most disease modeling, we are still averaging across cell types where the disease-driving signal is in a minority population.

**Right time.** Perturbation readouts at 24 hours look completely different from readouts at 6 hours or 3 days. Time is a first-class variable that most datasets flatten. This is why real-time, live-cell dynamics data is so valuable and so rare.

**Right controls.** Failed drug trials are not published. Cell-line experiments that did not work are not deposited. The training data is systematically biased toward the successes, which means your model learns "here is what worked" without learning "here is what did not." My friend Farhan at [Absentia](https://absentia.bio/) uses the enormous natural-compound safety set as a giant positive control. That kind of reframing of what counts as training data is more powerful than adding more of the same.

**Right metadata.** Every point above is impossible without good metadata. Which cell line. Which protocol. Which donor. Which passage number. Which vendor lot. Which day. This is unglamorous work and it is the actual bottleneck.

## Where I strongly agree with Koller

None of the above changes the core Koller take. She is right that:

- AI has not drugged the undruggable. KRAS breakthroughs are decades of structural biology and medicinal chemistry, not AI.
- Disease-to-mechanism is where the field should be putting more brains and dollars.
- Molecular design has been the flashy investment target, but it is downstream of the harder problem.
- Better clinical trials come from better biology, not from operational optimization.

These are the right calls. Where I differ is the diagnosis of what "better biology" needs. She frames it as coverage of the biological state space (more data). I frame it as quality, context-match, and metadata rigor (better data). These are not the same fix and they demand very different investments.

## What this means for how I evaluate AI-for-drug-discovery companies

When I look at a new AI drug discovery pitch now, three questions dominate.

- **What is the specific biological question, and does the training data actually address it?** Not "we have 10 million cells." Which cells, from which patients, in what condition, with what metadata? A curated 100K-cell dataset with clean labels and matched clinical outcomes is worth more than 10M cells scraped from disparate sources.
- **What decision does the model help make, and can you measure whether it made it better?** Following the Nature Reviews paper's framing. If the model does not measurably improve a real drug discovery decision (target selection, lead prioritization, patient stratification), it is a science project, not a therapeutic asset.
- **What is the loop between data generation and model updating?** The winning platforms will be the ones that generate their own targeted data in response to what the model does not know. This is closer to Koller's "strategic experimentation" than to "bigger public atlas."

The gap between "AI drug discovery" as a marketing story and AI drug discovery as a real capability is closing, but slowly. Both the Nature Reviews paper and Koller's essay are honest markers of where we are. Reading both together is the most efficient two-hour investment you can make this month if you work in this space.

If you push back on my framing of data quality vs quantity, or if you have counter-examples where scale alone did solve the problem, I want to hear them. Find me on [X](https://x.com/tangming2005) or [LinkedIn](https://www.linkedin.com/in/ming-tommy-tang-40650014/), or leave a comment below.

## Further reading

- Bender et al. Artificial intelligence in drug discovery — what it is, where we stand and the path forward. *Nature Reviews Drug Discovery*, Aug 2026. [Article](https://www.nature.com/articles/s41573-026-01496-2)
- Daphne Koller. Drug Discovery Has No Magic Wands. *a16z*, Aug 2026. [Essay](https://www.a16z.news/p/drug-discovery-has-no-magic-wands)
- Pat Walters. "Please Stop Fishing in the Bathtub" on AI drug discovery benchmarking. [Post](https://patwalters.github.io/Please-Stop-Fishing/)
- ISMB 2026 industry workshop (co-hosted with Shirley Liu, includes Anthony Philippakis panel). [Workshop site](https://ismb-2026-industry-workshop.vercel.app/)
- My LinkedIn post on the hammer-and-nail analogy from that panel. [LinkedIn](https://www.linkedin.com/feed/update/urn:li:share:7491489595222667264/)
- Kedzierska et al. Deep-learning-based gene perturbation effect prediction does not yet outperform simple linear baselines. *Nature Methods*, 2025. [Paper](https://www.nature.com/articles/s41592-025-02772-6)
- Anshul Kundaje on virtual cell models. [X thread](https://x.com/anshulkundaje/status/2034317728878907546)
- My earlier take: [Has AI Changed the Course of Drug Development? Three Years Later](/post/has-ai-changed-drug-development-3-years-later/)
- My biotech data strategy post: [How to Build a Data Strategy for Your Biotech Startup](/post/biotech-data/)
- My LinkedIn on real-time biology: [Post](https://www.linkedin.com/posts/%F0%9F%8E%AF-ming-tommy-tang-40650014_unlocking-the-true-potential-of-ai-and-activity-7448727853799497729-ixK1)
- My LinkedIn on why data is the bottleneck (PI3K/AKT signaling example): [Post](https://www.linkedin.com/feed/update/urn:li:share:7492576757078781952/)
