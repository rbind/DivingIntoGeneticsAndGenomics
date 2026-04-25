---
title: "Has AI Changed the Course of Drug Development? Three Years Later"
description: "A bioinformatics director's honest take on AI in drug development, three years after the original post. Updates on Recursion, BenevolentAI, Verge Genomics, plus what's actually working in the clinic and where the field is still stuck."
author: Ming Tang
date: '2026-04-24'
slug: has-ai-changed-drug-development-3-years-later
categories:
  - bioinformatics
  - genomics
tags:
  - AI
  - LLM
  - drug-development
  - clinical-trials
  - single-cell
  - bioinformatics
  - tutorial
math: false
header:
  caption: ''
  image: ''
---

Almost three years ago I wrote [Has AI Changed the Course of Drug Development?](/post/has-ai-changed-the-course-of-drug-development/). The post was cautiously skeptical. AlphaFold was the headline. BenevolentAI had just flopped a Phase 2a in atopic dermatitis. Verge Genomics' VRG50635 was about to enter the clinic. I argued that data quality was the real bottleneck, not algorithms.

Time to revisit that.

I now lead epigenomics bioinformatics at AstraZeneca and I use Claude Code daily. Some of what I predicted held up. Some did not. And the field has split into two clear lanes that were already there but are obvious now.

## The two buckets

When I evaluate AI solutions in drug development, I put them into two buckets:

1. AI touching the **process** of drug development.
2. AI touching the **biology**.

The process bucket includes clinical trial operations, statistical programming for FDA submissions, pharmacovigilance, literature monitoring. Stuff that looks more like text and structured data engineering. The biology bucket includes target identification, lead generation, lead optimization, biomarker discovery, toxicity prediction. Stuff where the underlying signal is noisy, sparse, and depends on context.

These two buckets have very different ROI right now. I'll get to that.

A note on terminology. When people say "AI" today they mostly mean large language models, which is a specific kind of deep learning model using a transformer architecture. For chemistry and biology applications, the architecture varies. CNN works well for image-based tasks. Diffusion models work well for de novo protein design. Linear regression and random forest still win surprisingly often when the data is moderate. I'll keep using "AI" as the umbrella term, but the relevant question is always: what model on what data, evaluated against what baseline.

## Where we were in 2023

Quick recap of the original post:

- AlphaFold2 was producing accurate protein structures, but Derek Lowe argued structural prediction was rarely the rate-limiting step in drug discovery. I agreed.
- VRG50635 from Verge Genomics had announced positive Phase 1 (safety) data. I noted Phase 1 is a low bar.
- BenevolentAI had laid off 180 staff after their atopic dermatitis trial flopped.
- Recursion was the standard-bearer for high-throughput cell imaging plus ML.
- I argued the real barrier was data: heterogeneous, siloed, and missing failed trials.

So how did those calls age?

**AlphaFold:** Right call. Three years later, structure prediction is faster and cheaper but I cannot point you to a clinical success that happened because of AlphaFold that would not have happened without it.

**Verge:** Phase 1b enrollment for VRG50635 in ALS completed in August 2024. The company has been publishing on digital biomarkers from the run-in period, but there are reports the Phase 1b did not deliver the efficacy signal they hoped for. ALS is brutal. Even non-AI drugs fail there.

**BenevolentAI:** The cautionary tale got worse. Another 30% layoff. They closed the US site. They are planning to delist from Euronext Amsterdam. The company is restructuring through 2027 and trying to out-license assets.

**Recursion:** Merged with Exscientia in November 2024 in an all-stock deal worth about `$688M`. Post-merger they trimmed the pipeline to roughly five clinical programs. Their lead asset REC-1245 (an RBM39 degrader) is reading out Phase 1 dose escalation data this year. 

Recursion is also still learning the biology side of things. The company built its platform on cell painting, the elaborate 5-6 dye fluorescent staining technique. They have now largely [switched to plain brightfield imaging](https://www.lesswrong.com/posts/5SsSZx5dMkRksrT85/why-recursion-pharmaceuticals-abandoned-cell-painting-for) because deep learning models work essentially as well on the unstained images, and brightfield opens the door to live-cell time-lapse data that the phototoxic dyes prevented. 

The hand-crafted features were not needed once the models got good enough. Whether the time-lapse data will actually translate into better drug discovery remains an open question, by their own admission.

The pattern: AI-first drug discovery companies are consolidating, narrowing pipelines, and partnering earlier. The "we will run drug discovery as a software loop" pitch has not generated a wave of approvals.

## AI on biology: what actually moved

### Small molecules

AI for small molecule design is real and useful. You can search a much bigger chemical space, optimize properties like solubility and PK, and generate candidates faster. But you still have to test them in cells, in animals, and in patients. Faster ideation does not eliminate the wet-lab loop. It speeds up the early funnel.

The unsexy truth: most of the wins from AI in small molecule design are incremental. They show up as faster lead optimization cycles inside big pharma, not as a string of new approvals. That is fine. Incremental still matters.

The exception worth calling out is Insilico Medicine. Their TNIK inhibitor rentosertib (ISM001-055) for idiopathic pulmonary fibrosis was discovered and designed end to end with their Pharma.AI generative AI platform. The Phase 2a results in 71 patients were published in [Nature Medicine in June 2025](https://www.nature.com/articles/s41591-025-03743-2) and showed a dose-dependent improvement in forced vital capacity (a 98.4 mL gain at the high dose versus a 62.3 mL decline on placebo). That is the first clinical proof of concept I have seen for a fully AI-discovered drug. Insilico went public on the Hong Kong Stock Exchange in December 2025, raising about `$293M` in the largest Hong Kong biotech IPO of the year, and signed an `$888M` partnership with a French pharma days later. It is one data point. I am eager to see more.

### De novo antibody design

Three years ago Absci was claiming de novo antibody design from scratch and people on Twitter were arguing about it. The skepticism was warranted then. It is less warranted now.

The Baker lab's [RFdiffusion paper in Nature](https://www.nature.com/articles/s41586-025-09721-5) (2025) showed atomically accurate de novo design of antibodies, including full heavy and light chain pairs. Mid-nanomolar affinities against targets like *C. difficile* toxin B, confirmed by cryo-EM. That is the architecture used in actual antibody drugs.

Absci has since moved their AI-designed anti-TL1A antibody ABS-101 into the [clinic](https://www.absci.com/our-pipeline/). They dosed the first healthy volunteers in a Phase 1 trial for inflammatory bowel disease in May 2025. Interim Phase 1 results in the second half of 2025 showed an extended half-life compared to first-generation anti-TL1A competitors, no serious adverse events, and no anti-drug antibody impact on PK. ABS-101 is the first AI-de-novo-designed antibody I have seen actually dosed in humans. I am waiting for the readout that matters, efficacy in patients, but the trajectory is real.

This is one of the few areas where I have updated my prior since 2023.

### KRAS, the obvious milestone

I want to call out one specific clinical win because it shows what real progress looks like.

Daraxonrasib (RMC-6236) from Revolution Medicines is a pan-RAS inhibitor. It hits multiple RAS mutations including G12D, G12V, G12R, and Q61X, which matters because PDAC is driven by KRAS mutations in over 90% of cases and the G12C-specific drugs that came first barely touched PDAC. Pancreatic ductal adenocarcinoma has been the graveyard of oncology drug development for decades. KRAS was famously "undruggable" until very recently.

Daraxonrasib is different. In a Phase 1/2 study at Memorial Sloan Kettering led by Dr. Eileen O'Reilly, 38 previously untreated PDAC patients receiving daraxonrasib showed a 47% response rate, 71% progression-free survival at 6 months, and 83% overall survival at 6 months ([source](https://www.mskcc.org/news/new-kras-targeted-therapy-shows-promise-against-pancreatic)). The drug is an oral pill taken once daily, with fewer side effects than chemotherapy. Dr. O'Reilly called it "potentially going to be a landmark shift in how we treat pancreatic cancer." The FDA has granted breakthrough therapy designation in the previously-treated metastatic setting based on separate Phase 3 RASolute data.

How much of that is AI? A modest amount. Revolution Medicines uses computational chemistry and structural biology heavily, but this is a story of biology first, chemistry second, AI as a productivity multiplier. The point is that real progress in oncology is happening in the targets we already understand, made tractable by new modalities.

### Toxicity prediction

In my experience, the dominant reason clinical-stage drugs fail is safety. And the clinical-stage failures are only the visible tip. Many promising molecules get killed for toxicity well before they reach pre-clinical IND-enabling studies, in early in vitro safety screens, hERG assays, hepatotoxicity panels, and exploratory tox in rodents. Those programs never make it to a press release. The published numbers say toxicity accounts for roughly 30% of clinical failures and lack of efficacy 40 to 50%. In my own experience, I find safety failures are a bigger share than the textbooks claim, because efficacy failures often have a hidden tox component (you cannot dose high enough to show efficacy because the safety window is too narrow).

You can use AI to design a molecule that avoids off-target toxicity. The hard problem is on-target toxicity. That is biology, not chemistry. If your target is required for normal physiology, no amount of clever molecular design will save you.

Companies are tackling this. I just learned that [Axiom Bio](https://www.axi.om/) raised `$200M` Series A to predict toxicity using AI. Axiom builds models of human drug response. But our body is an organic system. Drug concentrations fluctuate with PK/PD, distribution depends on tissue, and the relevant cell types may not even be in your assay. I am not convinced cell lines or organoids can predict the human safety profile in the way the field needs. They predict some things and miss others, and you do not know which until you dose a person.

The deeper problem is sparse data. You cannot train a great model on a small list of clinical failures, and the true positives (drugs that failed for tox reasons) are a fraction of the data you would need.

My friend Farhan at [Absentia](https://absentia.bio/index.html) takes a smart angle on this. All the natural compounds in our food are a giant positive control set for "this is safe in humans at the doses humans actually consume." If you train against that, you have orders of magnitude more data than the failed-drug databases give you. It does not solve the on-target tox problem, but it is the cleverest reframing of the data sparsity issue I have seen.

The other complication is that the ultimate test for safety is human, but the regulatory path forces you through animals first. Pharma companies run tox studies in mice, rats, dogs, and monkeys, and any signal in those species can stall a molecule before it reaches the clinic. BRD4 inhibitors, for example, look much more tolerable in mice than in humans. Aspirin is the reverse. If aspirin had to clear modern animal tox gates today, it would never have become a drug. The animal tox package is a checkbox in the regulatory process, not a clean readout of human biology. AI cannot fix this directly. The closest thing to a real fix would be high-throughput, multi-tissue human cell models that have actually been validated to recapitulate the kinds of organ-level tox you see in patients, and we are not there yet.

### New target identification (mostly skeptical, with one real exception)

I personally hold doubts on AI for de novo target identification. There are about 20,000 protein-coding genes in the human genome. If a target was easy to find by mining public data, somebody would have found it.

Most marketed cancer drugs go after well-characterized oncogenes: HER2, EGFR, KRAS. TP53 is a tumor suppressor, the most frequently mutated gene in cancer, and we still do not have a good drug against it. The well-known transcription factor drivers (MYC, MYB, several others) have been undruggable for decades not because we did not know about them, but because the protein surface does not have a pocket. New modalities are how we are getting at them. PROTACs degrade a target instead of inhibiting it. RIPTACs (regulated induced proximity targeting chimeras) recruit a co-essential protein to enforce selectivity. ADCs (antibody-drug conjugates) deliver a toxic payload only where the antigen is high.

The exception worth flagging is [GV20 Therapeutics](https://gv20tx.com/pipeline). My mentor [Shirley Liu](https://en.wikipedia.org/wiki/Xiaole_Shirley_Liu) is the co-founder and CEO. Their AI platform decodes patient B-cell repertoires to discover novel target-antibody pairs simultaneously, and it surfaced IGSF8 as an innate immune checkpoint, a target with very little prior literature attention. They published the discovery in [Cell in 2024](https://pubmed.ncbi.nlm.nih.gov/38657602/). Their lead antibody GV20-0251 is, as far as I can tell, the world's first AI-designed antibody against an AI-discovered target to enter the clinic. It is in a Phase 1/2 trial (NCT05669430) in advanced solid tumors, with a clinical collaboration to test it in combination with Merck's KEYTRUDA. At [ASCO 2025](https://ascopubs.org/doi/10.1200/JCO.2025.43.16_suppl.2531) they reported partial responses as monotherapy in two metastatic melanoma patients with primary resistance to anti-PD1, which is a hard population. So my prior on AI target ID is not that it is impossible. It is that the bar to do it well is high, and you need a specific data modality (B-cell responses, in GV20's case) that is not just public expression data.

Where AI is genuinely useful here is also downstream. Once you have a target and a mechanism, deep learning models trained on IHC images can quantify protein abundance across patient samples in ways that are hard to do by eye. CNNs work well for this. That is operational, not discovery.

## The data problem has not gone away

This was the punchline of the 2023 post. It is still the punchline.

Why can we train large language models on essentially the entire text of the internet and get models like Claude Opus that produce coherent multi-paragraph responses, but we cannot do the equivalent for biology?

Because text on the internet is a very forgiving training set. The signal-to-noise ratio is high. The "labels" are implicit in the text itself. When the model mispredicts the next token by a small amount, the loss gradient is reasonable.

Biology data is the opposite. The Gene Expression Omnibus (GEO) is one of the most-used public repositories. Labs deposit their data there when they publish. My friend Jinfeng Zhang once analyzed all of GEO and told me he found as high as 25% sample mislabeling. A lab's "MCF7 breast cancer cells" might actually be HeLa. This is not exotic. Cell line misidentification is a known problem in biology.

Even when the labels are right, the data is not just rows and columns. It was generated in a wet lab by a human. Cells were in a particular state. The treatment concentration was a specific number. The time point was Tuesday at 3pm. Even data from the same lab on the same cell line at different times can show meaningful batch effects. How you normalize that data before feeding it into a model matters more than which model you choose.

GEO has a lot of data. Most of it is not useful for the question you actually want to answer.

OK, you might say, let's train a single foundation model on all single-cell data and apply it to specific contexts. The recent [Nature Methods paper by Kedzierska et al.](https://www.nature.com/articles/s41592-025-02772-6) tested this directly. They benchmarked five foundation models and two other deep learning models against simple linear baselines for predicting transcriptome changes after gene perturbation. The deep learning models did not outperform the linear baselines.

That is a damning result. Not because deep learning is wrong in principle, but because the field has been promising foundation models will deliver biology insight at scale for several years now, and the simplest possible baselines are matching them.

[Anshul Kundaje](https://x.com/anshulkundaje) at Stanford has been the loudest honest voice on this. In a [March thread](https://x.com/anshulkundaje/status/2034317728878907546) he laid out the argument plainly. The current generation of "virtual cell models" trained on scRNA-seq atlases (scGPT, scFoundation, GEARS) are basically doing fancy nearest neighbor search. Recent benchmarks show that a literal nearest neighbor lookup using only prior-knowledge embeddings outperforms gigantic pretrained models on perturbation prediction. His exact phrasing was hard to ignore: "stop insulting the centuries of effort put in by biologists & computational biologists who built those prior knowledge networks & literature that u piggy back on by calling ur nearest neighbor look up engine a virtual cell." If your model is learning biology, show the known and novel regulatory logic it is uncovering. Otherwise it is pattern matching dressed up as biology.

The companion point in his thread was about training data. Cell line and mouse data are not useless. Cis-regulatory logic is remarkably conserved between human and mouse, and a model trained on resting and stimulated T-cell line data can transfer to in-vivo T-cells. The training data should match the use case. The pipe dream is generating data in arbitrary contexts and hoping a giant model will solve all other arbitrary contexts. The amount of data required to actually pull that off would be astronomical. We also have to start conditioning on spatial context. Cells behave differently in different microenvironments and ignoring that makes any perturbation prediction physiologically suspect.

His broader work makes the same case in regulatory genomics: lightweight interpretable models like his lab's ChromBPNet keep outperforming large DNA foundation models on variant effect prediction and regulatory sequence design, with a fraction of the compute. Model size is not a substitute for relevant data, and prior knowledge is not the same as causal mechanism.

Single-cell RNA-seq has its own artifacts: heat shock from the cell isolation protocol, transcriptional drift, dropout. The cells you end up sequencing are not the cells you started with.

There is also a fundamental issue with the kind of data we collect. We destroy the cells and read out their final state with NGS. The cells are dead by the time we get the matrix. The biology that mattered, the dynamics that produced the state, is lost. I wrote about this on [LinkedIn recently](https://www.linkedin.com/posts/%F0%9F%8E%AF-ming-tommy-tang-40650014_unlocking-the-true-potential-of-ai-and-activity-7448727853799497729-ixK1): until we get high-throughput real-time biology, we will keep training models on snapshots of dead cells.

The obvious response to all this is to generate better data with lab automation. [Medra](https://www.medra.ai/) is the company everyone is posting about right now. They raised a `$52M` Series A in December 2025 to build "Physical AI scientists," robots that run wet-lab experiments end to end, with a partnership signed with Genentech. The pitch is compelling: standardized, reproducible, machine-readable biology at scale. The training data problem solves itself if a robot generates the data.

I want to believe this and I am skeptical. A friend at Ginkgo Bioworks gave me the unvarnished take. Lab automation looks great in a demo. The reality on the ground is that the robots are very expensive, they are not as adaptive as a trained human, and you can really only automate parts of the workflow cleanly. Setup that takes a human one hour can take three hours on the robot. You only win when the protocol is so high-volume and so standardized that the upfront cost amortizes across thousands of runs. For exploratory biology, the human is still faster.

The Ginkgo story is itself a cautionary tale. They have lost roughly `$6 billion` since 2020, laid off about a third of staff, and are pivoting hard toward selling lab automation infrastructure (their Nebula platform) instead of running synthetic biology programs themselves. The company that scaled biology automation faster than anyone has spent a decade learning what does not work. That history should sit somewhere in the back of your mind when you read Medra's funding announcements. The bet is real and it might pay off, but the "AI scientist that replaces the wet lab" pitch has been made before, and the lab is harder than it looks.

## Clinical trials are still where AI struggles most

Phase 1 oncology trials have around 64 patients. Phase 2 has a few hundred. You profile them with IHC, flow cytometry, RNA-seq, sometimes WES. That is sparse data by deep learning standards. You cannot train a transformer on 64 patients.

You can use AI for biomarker discovery and patient stratification, and that work is real. But what is a biomarker, honestly? Most of the time it is the target itself or its ligand. PD-1 expression for anti-PD-1 therapy. HER2 amplification for trastuzumab. KRAS G12C for sotorasib. We are not finding profound new biomarkers with AI. We are validating that the obvious biomarker is the right one.

You can use vendors like Tempus that hold massive multi-modal cohorts. But once you filter to a specific cancer type, a specific mutation, patients with a specific pretreatment history, with matched pre- and post-treatment biopsies, your N collapses. And some of the patients do not have pre/post matched samples. And some sites use different definitions of "pre" and "post." If you have done the dirty work in the trenches, you know what I mean. I [wrote about pre/post definition issues](https://www.linkedin.com/posts/%F0%9F%8E%AF-ming-tommy-tang-40650014_1-you-think-clinical-trial-genomics-is-simple-activity-7342540627386986496-TR3r/) and it is still one of the most underappreciated sources of error in clinical genomics.

Single-cell stress artifacts compound the problem. The act of dissociating a tumor for scRNA-seq induces a heat-shock signature. Some "biology" you see in your model is just sample handling.

## AI on the process: the quieter wins

This is where I think AI is actually delivering value today, and the headlines have not caught up.

**Clinical trial coordination.** Trials require coordination across many sites, sponsors, CROs, and the patients themselves. Doctors enter data into trial databases when patients visit. Mistakes happen. Typos happen. Inconsistent date formats happen. AI is excellent at flagging these errors and routing them back to the site for correction. This is the kind of unsexy back-office work that compounds.

**Statistical programming for FDA submissions.** Stat programming has rigid standards: ADaM and SDTM data structures, a defined set of tables, listings, and figures (TLFs). FDA expects specific input and output formats. To be clear, LLMs themselves are not deterministic. Run the same prompt twice and you get different outputs. But the TLF task is deterministic. There is one right answer for a given dataset and protocol. The pattern that works is to use the (non-deterministic) LLM to generate the (deterministic) SAS or R code, then run that code against the data and verify the output matches the spec. The variability lives in the code generation step where it is cheap to retry, not in the final output that gets submitted to FDA. The harder part is the audit trail and the regulatory scrutiny, but the actual code generation is a win. [Hill Research](https://www.hillresearch.ai/) is one example of a company building exactly this. Their TriClick platform automates CDISC annotation, statistical programming, and TLF generation with audit trails baked in. They claim 2,000x faster TLF generation and say two of the top three pharma companies use them. That kind of ROI is hard to ignore.

**Pharmacovigilance literature monitoring.** PV teams have to scan published literature for adverse events related to their products. That is a text mining problem. LLMs are much better than humans at sifting through thousands of papers for relevant safety signals. My friend Jinfeng Zhang's company Insilicom built [IPV](https://pv.insilicom.com/), an AI-powered adverse event literature monitoring platform that they say reduces the manual review workload by at least 10x. This is the same Jinfeng who told me about the GEO mislabeling problem. He has been working on text mining for biomedical literature for years and the PV use case is a natural fit.

None of these get a press release. All of them are saving time and reducing cost right now. Drug development is ultimately a process problem. AI for the process is the bet I would make if I were running a biotech today.

## My own workflow

In my day job as a bioinformatics director I use Claude Code daily. My workflow:

1. Enter plan mode.
2. Give the LLM as much context as possible: slide decks describing the rationale and experimental design, prior analysis notebooks, the actual data files I want to use.
3. Frame the question I want answered with the data.
4. Let Claude write an Rmd file, knit it to HTML, and read the output.

It works well. The Rmd files Claude produces are decent. The plots are usable. It is much faster than typing the commands myself. The code quality is mostly good.

But you have to proofread. Two specific failure modes I have hit recently:

**Hallucinating from outdated training data.** A colleague asked Claude about RIPTACs. The model had no idea what RIPTAC was and produced confident-sounding nonsense. New modalities are not in the training data. If the field moved in the last six months, the LLM does not know.

**Composite errors that look right.** A colleague of mine asked an AI to combine TCGA tumor expression data with GTEx normal tissue expression to find tumor-specific targets. It returned a clean list. Some of the targets had absurd expression patterns, sky-high in tumor and apparently zero in normal. When we dug in, the two datasets were using different gene ID versions. When the model could not match a gene ID in GTEx, it filled in zero, then "discovered" that the gene was tumor-specific. The fix took 30 seconds once we spotted it. The bug would have made it into a target list if no one had caught it.

**Z-score axis matters.** I asked Claude to make a heatmap of ChIP-seq peak signal across antibodies. The default behavior was to z-score by row (per peak across samples). For my data, that was wrong. The columns were different antibodies, and antibody affinity varies, so comparing absolute binding strength across antibodies is not biologically meaningful. I wanted the relative signal across peaks within each antibody, which means z-scoring by column. Claude scaled the wrong axis. Once I told it which axis to scale, it fixed it instantly. But understanding why one z-score direction is right and the other is wrong requires biology and assay-level knowledge. The LLM did not have it.

The pattern across all of these: AI accelerates the work, but you still need to evaluate the output line by line. If you catch the error and feed it back, the model adjusts fast. If you do not catch it, the bad result becomes the answer of record.

So yes, AI increases my efficiency. It does not replace judgment.

## Three years from now

Where do I think this goes?

The biology AI story will keep crawling forward. Foundation models will keep getting bigger and the linear baselines will keep beating them on out-of-distribution tasks. We will see a few more high-quality wins from generative protein design, mostly in antibodies and nanobodies. Small molecule design will continue to compound inside big pharma without producing headline approvals.

The process AI story will compound faster than people expect. Statistical programming, trial monitoring, regulatory documentation, pharmacovigilance: the cost savings here are real and the regulatory bar is achievable. By 2029 I expect every Phase 3 trial of any size to have AI in the operations stack, and I expect the FDA to have explicit guidance on it.

The biggest unlock would be cheap, high-throughput, real-time biology data. Live-cell imaging at scale. Perturbation screens with kinetic readouts. Multi-modal single-cell that includes time. If somebody figures out how to generate that data at the volume LLMs need, the biology AI story unlocks. Until then, we are training on dead cells.

The 2023 take was: AI has not revolutionized drug development yet. The 2026 update is: AI has not revolutionized drug development yet, but it is steadily transforming the parts of drug development that nobody writes articles about. That is fine with me. The biology will catch up when the data does.

What is your take? Where are you seeing AI actually move the needle in drug development, and where are you seeing it overpromise? Find me on [X](https://x.com/tangming2005) or [LinkedIn](https://www.linkedin.com/in/ming-tommy-tang-40650014/), or leave a comment below.

## References

**Companies and pipeline updates**

- Recursion / Exscientia merger announcement: [Recursion IR press release](https://ir.recursion.com/news-releases/news-release-details/recursion-and-exscientia-enter-definitive-agreement-create)
- Recursion abandoning cell painting for brightfield imaging: [LessWrong post by Charles Baker](https://www.lesswrong.com/posts/5SsSZx5dMkRksrT85/why-recursion-pharmaceuticals-abandoned-cell-painting-for)
- BenevolentAI layoffs and US site exit: [Fierce Biotech](https://www.fiercebiotech.com/biotech/benevolentai-lays-30-staff-exits-us-site-funding-gap-looms)
- BenevolentAI plans to delist from Euronext Amsterdam: [Drug Discovery World](https://www.ddw-online.com/benevolentai-announces-redundancies-as-it-plans-to-delist-and-refocus-32869-202412/)
- Verge Genomics digital biomarker findings from VRG50635 Phase 1b run-in: [Verge Genomics announcement](https://www.vergegenomics.com/news-blog/verge-and-collaborators-identify-digital-clinical-biomarker-endpoints-sensitive-to-short-term-disease-progression-in-the-pre-treatment-run-in-period-of-vrg50635-proof-of-concept-als-clinical-trial)
- Verge Genomics VRG50635 ClinicalTrials.gov record: [NCT06215755](https://clinicaltrials.gov/study/NCT06215755)
- Insilico Medicine rentosertib (ISM001-055) Phase 2a in IPF, Nature Medicine 2025: [paper](https://www.nature.com/articles/s41591-025-03743-2)
- Insilico Medicine Hong Kong IPO: [Pharmaphorum coverage](https://pharmaphorum.com/news/insilico-ends-2025-293m-hong-kong-ipo)
- GV20 Therapeutics pipeline: [https://gv20tx.com/pipeline](https://gv20tx.com/pipeline)
- GV20 IGSF8 discovery (Cell 2024): [PubMed](https://pubmed.ncbi.nlm.nih.gov/38657602/)
- GV20-0251 ASCO 2025 melanoma monotherapy data: [JCO abstract](https://ascopubs.org/doi/10.1200/JCO.2025.43.16_suppl.2531)
- GV20-0251 + KEYTRUDA collaboration announcement: [PRNewswire](https://www.prnewswire.com/news-releases/gv20-therapeutics-announces-clinical-trial-collaboration-to-evaluate-gv20-0251-a-first-in-class-antagonist-antibody-against-the-novel-immune-checkpoint-igsf8-in-combination-with-keytruda-pembrolizumab-302120896.html)

**KRAS / Daraxonrasib**

- Daraxonrasib in untreated PDAC, MSK Phase 1/2 results: [Memorial Sloan Kettering news](https://www.mskcc.org/news/new-kras-targeted-therapy-shows-promise-against-pancreatic)
- FDA breakthrough designation for daraxonrasib: [Targeted Oncology](https://www.targetedonc.com/view/daraxonrasib-earns-fda-breakthrough-status-in-pancreatic-cancer)
- Phase 3 RASolute 302 trial: [ClinicalTrials.gov NCT06625320](https://clinicaltrials.gov/study/NCT06625320)

**Antibody and protein design**

- Atomically accurate de novo antibody design with RFdiffusion: [Nature 2025](https://www.nature.com/articles/s41586-025-09721-5)
- Baker lab announcement on antibody design with RFdiffusion: [Institute for Protein Design](https://www.ipd.uw.edu/2025/11/rfantibody-in-nature/)
- Absci ABS-101 (anti-TL1A) Phase 1 first dose (May 2025): [Absci press release](https://investors.absci.com/news-releases/news-release-details/absci-announces-first-participants-dosed-phase-1-clinical-trial)
- Absci pipeline page (ABS-101 listed in Phase 1 for IBD): [https://www.absci.com/our-pipeline/](https://www.absci.com/our-pipeline/)

**Toxicity prediction**

- Axiom Bio: [https://www.axi.om/](https://www.axi.om/)
- Absentia: [https://absentia.bio/](https://absentia.bio/index.html)

**Process AI / clinical trial operations**

- Hill Research (AI-powered statistical programming, CDISC annotation, TLF generation): [https://www.hillresearch.ai/](https://www.hillresearch.ai/)
- Insilicom IPV (AI-powered pharmacovigilance literature monitoring): [https://pv.insilicom.com/](https://pv.insilicom.com/)

**Lab automation**

- Medra (Physical AI scientists, $52M Series A, Dec 2025): [https://www.medra.ai/](https://www.medra.ai/) and [Bloomberg coverage](https://www.bloomberg.com/news/articles/2025-12-11/medra-raises-52-million-to-speed-drug-discovery-with-ai-robots)
- Ginkgo Bioworks pivot to autonomous labs and restructuring: [Boston Globe](https://www.bostonglobe.com/2026/03/03/business/ginkgo-bioworks-pivot-ai-robots/) and [Fierce Biotech on layoffs](https://www.fiercebiotech.com/medtech/ginkgo-bioworks-lay-many-400-employees)

**Data and modeling limits**

- Kedzierska et al., Deep-learning-based gene perturbation effect prediction does not yet outperform simple linear baselines: [Nature Methods 2025](https://www.nature.com/articles/s41592-025-02772-6)
- Why 90% of clinical drug development fails (toxicity / efficacy breakdown): [PMC review](https://pmc.ncbi.nlm.nih.gov/articles/PMC9293739/)
- Anshul Kundaje March 2026 thread on virtual cell models, nearest neighbor critique, training data, and spatial context: [X thread](https://x.com/anshulkundaje/status/2034317728878907546)
- Anshul Kundaje September 2024 thread on scGPT, scFoundation, GEARS vs linear baselines: [X thread](https://x.com/anshulkundaje/status/1837327226200584610)

**My own posts**

- The 2023 original: [Has AI Changed the Course of Drug Development?](/post/has-ai-changed-the-course-of-drug-development/)
- Real-time biology and the limits of NGS snapshots: [LinkedIn post](https://www.linkedin.com/posts/%F0%9F%8E%AF-ming-tommy-tang-40650014_unlocking-the-true-potential-of-ai-and-activity-7448727853799497729-ixK1)
- Pre/post definition issues in clinical trial genomics: [LinkedIn post](https://www.linkedin.com/posts/%F0%9F%8E%AF-ming-tommy-tang-40650014_1-you-think-clinical-trial-genomics-is-simple-activity-7342540627386986496-TR3r/)
