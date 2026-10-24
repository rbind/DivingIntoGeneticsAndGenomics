---
title: "Is Bioinformatics Doomed? The Future of Bioinformatics in the AI Era"
description: "A teacher says bioinformatics is doomed because AI beats any entry-level analyst. My take on the future of the field, the foundations, and training beginners."
author: Ming Tang
date: '2026-10-24'
slug: is-bioinformatics-doomed-future-of-bioinformatics-ai
categories:
  - bioinformatics
  - education
tags:
  - AI
  - bioinformatics
  - education
  - career
  - statistics
  - linear-algebra
  - LLM
math: false
header:
  caption: ''
  image: ''
---

My friend Glen recently sent me a LinkedIn post about the future of bioinformatics. It was not a happy one.

The author teaches an intro bioinformatics course. Their argument went like this:

- Nothing in an intro course makes a newbie employable anymore.
- Entry-level positions are gone, because Claude is smarter and faster than any entry-level research assistant.
- If you are a student thinking about this field, stop. The field is doomed.
- Only experienced people are valued now. The way in is a PhD, mostly for people with a hard-science background. Biologists who learned to program should switch fields or do a wet/dry PhD.

I have been doing bioinformatics for 14 years, and teaching it is the thing I care about most. So this one hit me.

My short answer: AI is a tool. It makes coding much faster. It does not make the foundations less important. It makes them more important.

But the post also points at a real problem, and I do not want to wave it away. So let me go through it.

## What the bioinformatics community said

The post made its way to [r/bioinformaticscareers](https://www.reddit.com/r/bioinformaticscareers/comments/1x6z7ni/thoughts/) and got about 90 comments. A few stuck with me.

**It is not only AI.** The most upvoted comment said entry-level hiring is drying up in every field, blue or white collar, and pointed at interest rates. Others added cuts to research funding. Someone else noted that junior roles had been shrinking for close to 10 years, long before coding agents showed up.

**Teach people to define the problem and check the answer.** Several people said what I would have said. One said to teach algorithms and how to think, not how to install Python. A PhD student asked a simple question: who is running the AI? They use an agent every day to troubleshoot pipelines, but they still direct it, review the output and do the interpretation.

**AI misses weird data.** One person said AI can run a standard workflow fine, but it is not great at noticing when the data look wrong and knowing what to do about it.

**The honest counterpoint.** One commenter said that a single person with AI can now do the work of three juniors. That is the part that worries me too. More on that below.

**The optimist.** A long-time member of the community pointed out that bioinformatics has been riding the NGS wave for 20 years. The pipelines matured, so fewer people are needed to build them. But after the 2009 crash, by 2012 HiSeq machines were everywhere and there was a shortage of bioinformaticians. New technology brings new demand.

I agree with parts of all of them.

## AI is a tool, and it amplifies what you already know

Let me be clear: I am bullish on AI. I use Claude Code almost every day, at work and at home. Every crisis is also an opportunity, and the opportunity here is real. AI lets me do things I could not do before.

**I revived old tools.** [BETA](/post/reviving-beta-chip-seq-rna-seq-integration-python3/), the ChIP-seq and RNA-seq integration tool from Shirley Liu's lab, was stuck on Python 2. It took me about 8 hours of prompting Claude Code to port it to Python 3, publish it on PyPI and set up CI/CD. I did the same for [ROSE2](https://github.com/crazyhottommy/ROSE2), the super-enhancer caller. The old version took about 9 hours on 22K enhancers on hg38. The new one takes about 12 minutes. Gene mapping went from 6 hours to 12 seconds.

**I build web apps now.** I am not a software engineer. But I can build a Shiny or Streamlit app in a couple of days, something that might take a good software engineer two weeks. The app wraps a deterministic R or Python script, so a wet-lab scientist can upload the spreadsheet from their machine and get reproducible results.

About a year earlier, I had spent 40 hours building a web app on Replit. The authentication logic kept breaking, and when I tried to add features, the whole thing fell apart. The models got better, sure. But I also learned to plan first, give clear specs, and review everything that comes back.

Here is the thing. In every one of these wins, I knew what the right answer looked like. I had used BETA for years. I knew what a super-enhancer hockey-stick plot should look like. AI made me faster at things I already understood.

AI will multiply your skills. But only if you have them first.

### Confidently wrong

The other side of the coin: AI answers your question no matter what. And it can answer it wrong, confidently.

One commenter in that Reddit thread, with 10 years of experience, spent a whole night correcting an AI on a simple analysis. It kept reporting anything with a small p-value as a finding, even though the prompt told it to check for confounding variables and Simpson's paradox first. Their point was that even a fresh grad should catch that.

I have hit the same thing. I wrote about three failures in my post on [whether AI has changed drug development](/post/has-ai-changed-drug-development-3-years-later/):

- An analysis that merged TCGA tumors with GTEx normal tissue, where mismatched gene ID versions turned into zeros and the genes looked "tumor-specific."
- A heatmap z-scored along the wrong axis.
- Confident nonsense about a new drug modality the model had never seen.

Each fix was quick once a human spotted the problem. If nobody spots it, the bad result becomes the answer of record.

Vague prompts to an AI agent produce confident garbage.

## The foundations matter more, not less

The skills that let me catch those errors are the same boring basics I have been preaching for years. AI actually raises the bar for them. You used to write the code and test it as you went. Now the code shows up in 30 seconds, and your job is to judge it.

Here is what I still tell every junior bioinformatician.

### 1. Biology and domain knowledge

The hard part is framing the question. Say you study estrogen receptor (ER) in breast cancer and androgen receptor (AR) in prostate cancer. AI will happily write a script for AR binding in a breast cancer cell line. Whether that question makes biological sense is on you.

The same goes for parameters. If you have never aligned reads, you will not think to ask whether the default settings fit your read length. You do not know to ask.

You cannot out-biology your wet-lab collaborators, so talk to them. Learn from them.

### 2. Unix

I learned Unix commands 14 years ago and got really comfortable after three or four years. I did not expect that skill to transfer to the AI era. It did.

Claude Code lives in the terminal. Knowing paths, the file system and how to put the right files where the agent can see them makes it far more powerful. And when a huge file breaks, you need `head`, `wc`, `awk` and `sort`. If you can't grep it, you can't fix it.

### 3. R or Python

Pick one and learn it well enough to read every line the AI writes. People tell me they do not need to learn a language anymore because they can ask AI. Fine, until it breaks and you have to figure out why.

AI can write you an awk command that runs fast and trims the wrong bases. Would you notice? See my post on [R or Python for bioinformatics](/post/r-or-python-for-bioinformatics/) if you are deciding.

### 4. Git and software basics

When an agent rewrites 12 files, `git diff` is how you see what changed, and git is how you roll it back.

[Kevin Libuit](https://www.linkedin.com/in/libuitkg/) made this point well when we talked recently. He does not use git much day to day anymore. But because he knows the concepts, he can tell his agent "make a dev branch, I want to test this feature" and coordinate it like a small dev team. The same goes for unit tests and code review. If you do not know a practice exists, you cannot ask the AI to follow it.

### 5. Statistics

Know the assumptions behind a test. Know why p < 0.05 on its own means little when you test 20,000 genes (you expect about 1,000 false positives by chance alone), and what [FDR and q-values](/post/understanding-p-value-multiple-comparisons-fdr-and-q-value/) do about it.

Learn linear models. They (and their generalized cousins) are behind DESeq2, limma and [batch effect correction](/post/batch-effect-correct-or-not-bulk-rnaseq/). An AI-generated DESeq2 script can make a beautiful volcano plot without knowing there is a batch effect in your design. That Simpson's paradox story above is a statistics problem, not a coding problem.

### 6. Linear algebra

Most of our data are just rectangular matrices: genes by samples, genes by cells, peaks by samples. PCA, SVD, NMF and other [matrix factorization](/post/matrix-factorization-for-single-cell-rnaseq-data/) methods all come from linear algebra.

My biggest regret is that I barely passed linear algebra in college. I wrote about [learning it properly years later](/post/learn-linear-algebra/). If you only want to start with one thing, start with [PCA](/post/pca-in-action/).

### 7. Machine learning

You do not need to train foundation models. But you should know what logistic regression with regularization, random forests and neural networks are doing, and how they fail through overfitting and data leakage.

Josh Starmer's [StatQuest](https://statquest.org/video-index/) videos and his *StatQuest Illustrated Guide to Machine Learning* are the best place to start. I have watched his PCA video more than 20 times.

### 8. Communication

You will explain results to biologists who do not care about your code. Writing a clear brief for an AI agent is the same skill as writing a clear brief for a junior analyst. Sloppy input, plausible-sounding waste.

None of this is new. I laid out a full learning path in [how I would learn bioinformatics from scratch](/post/bioinfo-roadmap/). The [hidden skills beyond programming](/post/hidden-skills-beyond-programming-for-computational-biology/) and the [stupid mistakes we all make](/post/stupid-mistakes-for-bioinformatics/) matter even more now.

Again, you do not know what you do not know. Without the basics, you cannot tell when the AI is hallucinating.

## What Kevin and I talked about

Kevin spent 10 to 15 years in infectious-disease pathogen genomics before moving into AI implementation. We talked for an hour, and a few of his points have stayed with me.

**The water heater.** Kevin's water heater acted up, and he asked AI to help him troubleshoot it. He went around in circles. AI told him to look here, then there. He could not judge any of the answers because he knew nothing about water heaters. He ended up calling a guy. That is what it looks like to use AI without domain knowledge, and it is exactly what a junior faces with a pipeline they have never run.

**When code is cheap, there is a premium on scope.** Kevin passed along this line from a go-to-market engineer he had talked to at Vercel. Figuring out what the problem is, and how to validate the result, becomes the most valuable skill. I think of AI as a hammer. You still have to find the nail. And sometimes you do not need AI at all. A deterministic Nextflow pipeline from FASTQ to counts, or a plain cron job, beats a probabilistic agent for routine steps.

**Struggle is how you grow.** Kevin compared it to building muscle: you have to strain the system. Before AI, the program did not run, and you had to figure out why. That pain is where the learning happened. Now you can get an answer without ever processing it in your own brain. I brought up GLP-1: it helps you lose weight, but you lose muscle too unless you still go to the gym. Skills can atrophy the same way.

**Accountability.** If your collaborator says the result is wrong and you ask the AI, it will say "you're right, I was wrong." The AI is not accountable. The person who ran the tool is.

## My real worry: who trains the next generation of bioinformaticians?

Here is the part I do not have a clean answer to.

We were all once beginners. I started as a wet-lab biologist. My first sequencing file crashed Excel. I practiced Unix during two-hour gel runs. When I asked a senior labmate for help, they refused to share their script.

I made every mistake in the book. I mixed up 0-based and 1-based coordinates. I hit off-by-one errors more times than I can count. It took me about 10 years to go from wet lab to bioinformatician, and it took Kevin about the same.

An entry-level job is someone paying you to make those mistakes in a place where someone else catches them. That is how judgment gets built.

Right now, many companies hire only senior people, and a lot of entry-level positions are simply not posted anymore. I understand why. One experienced person with AI can cover a lot of ground.

But every senior bioinformatician was once a junior. People like Kevin and me were lucky: we were trained before AI, and now we get to supercharge what we learned. If nobody hires juniors today, where do the experienced bioinformaticians come from in 10 years?

I do not think "get a PhD" is the whole answer either. A PhD is one apprenticeship model, and a good one: you take an idea, test it and understand it to an absurd degree. But not everyone can spend five to seven years on it, and we cannot expect that to be the only door.

Kevin described one model I liked. In a Virginia program he is involved with, they train early-career scientists by treating each cohort as if it were its own drug discovery company submitting a proposal to the FDA. Use any tool you want, AI included. But you have to build the argument, decide which experiments to run, and defend it when people push back. That is project-based learning with accountability built in.

I do not see an industry-wide solution yet. But I think companies, universities and senior people like me all have to work on it, not just complain that juniors are not ready.

## What I will keep doing

For students: do not stop. Yes, the path is harder than it was five years ago. But the people who succeed are the same kind of people who always succeeded: consistent, persistent, and always willing to learn.

- **Embrace AI as a tutor, not a shortcut.** Ask it to explain code line by line. Ask it to build you a learning plan. Kevin suggested reverse prompting: ask the AI to quiz you and assess what you actually understand.
- **Then go do a real project.** I learned more from analyzing real data during my PhD than from any course.
- **Use it to learn, not to skip learning.**

For seniors: if you're senior, share more. If you're junior, ask more. Be the person you needed when you were starting out.

As for me, my mission is to teach 1 million people bioinformatics, and AI does not change that. I will keep evangelizing the basics: the foundations, intuition, best practices and sanity checks. Again, you do not know what you do not know.

I put three years of these lessons into my upcoming book, *What They Don't Teach You in Bioinformatics*. [Join the founding-reader waitlist](https://divingintogeneticsandgenomics.kit.com/wtdt_bioinformatics?utm_source=website&utm_medium=blog_post&utm_campaign=book_prelaunch) and get a free 15-lesson sampler.

## Further reading

- The Reddit discussion: [r/bioinformaticscareers](https://www.reddit.com/r/bioinformaticscareers/comments/1x6z7ni/thoughts/)
- [How I Would Learn Bioinformatics From Scratch](/post/bioinfo-roadmap/)
- [Has AI Changed the Course of Drug Development? Three Years Later](/post/has-ai-changed-drug-development-3-years-later/)
- [AI in Drug Discovery: Data Quality, Not Quantity, Is the Bottleneck](/post/ai-drug-discovery-data-quality-not-quantity/)
- [Reviving BETA for Python 3](/post/reviving-beta-chip-seq-rna-seq-integration-python3/)
- [Great Things Take Time: How Decades of Effort Led to My Dream Career](/post/great-things-take-time-how-decades-of-effort-led-to-my-dream-career/)
- [StatQuest video index](https://statquest.org/video-index/)

If you are a student, what scares you most about entering bioinformatics right now? If you hire bioinformaticians, how are you training juniors in the AI era? Find me on [X](https://x.com/tangming2005) or [LinkedIn](https://www.linkedin.com/in/ming-tommy-tang-40650014/), or leave a comment below.
