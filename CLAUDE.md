# Blog Writing Guide for Chatomics (Diving Into Genetics And Genomics)

## Purpose

This guide provides instructions for creating SEO-optimized blog posts that maintain the authentic voice and practical focus of the Chatomics blog while maximizing search visibility and reader engagement.

---

## Voice & Tone Guidelines

### Core Voice Characteristics

**Conversational & Personal**
- Use first person ("I", "my") naturally - you're sharing your experience
- Write like you're explaining to a colleague over coffee
- Be direct and honest about challenges: "During my daily work with R for genomic data analysis, I encountered several instances that R gives me some (bad) surprises"
- Share your learning journey: "This was the first book that I used to learn unix, regex and python"

**Accessible Expert**
- Technical but not intimidating
- Explain the "why" behind recommendations, not just the "how"
- Acknowledge when things are confusing or counterintuitive
- Use phrases like "Keep in mind that...", "Note that...", "Thanks [Person] for pointing it out"
- Reference and credit other experts generously

**Problem-Focused & Practical**
- Start with the problem or use case
- Focus on gotchas, common mistakes, and real-world challenges
- Include working code examples that readers can actually use
- Use real datasets (GEO, public data) rather than toy examples
- Address the "devil in the details" that tutorials often skip

**Community-Oriented**
- Welcome reader feedback: "What's your favorite book that I have missed? Comment below!"
- Credit sources and colleagues by name
- Invite guest posts: "If you want to do a guest posting in my blog which gets 30k views per month, feel free to contact me"
- Share links to your compiled resources and GitHub repos

### Style Elements

**Section Headers**
- Use clear, descriptive H2/H3 headers with keywords
- Question-based headers work well: "Are the feature barcode whitelist the same for scRNAseq and scATAC?"
- Problem-focused headers: "The devil 1 and 0 coordinate system"

**Code Integration**
- Show code with clear comments
- Explain what the code does and why
- Include bash commands, R code, Python code as relevant
- Show both the solution and common mistakes

**Visual Aids**
- Include screenshots, plots, and diagrams when helpful
- Use tables for comparisons
- Reference figures clearly: `![](/img/posts_img/rnaseq_library.png)`

**Engagement Tactics**
- Ask questions to the reader
- Leave exercises: "I will leave this as an exercise for the reader"
- Encourage experimentation: "You can try..."
- Admit when answers aren't clear-cut: "There is no single best answer right now"

---

## SEO Optimization Strategy

### Title Optimization

**Priority Keywords (Use in titles)**
- single cell rna seq
- seurat tutorial
- bioinformatics tutorial
- scRNAseq analysis
- DESeq2
- ATAC-seq
- python bioinformatics
- r bioinformatics

**Title Formulas That Work**

**1. Problem-Focused (Excellent for traffic & engagement)**
```
[Number] [Common/Hidden] [Task] Mistakes (And How to Fix Them)
[Number] gotchas when [doing task]

Examples:
- Three gotchas when using R for Genomic data analysis
- 5 Common Single-Cell RNA-seq Analysis Mistakes (And How to Fix Them)
- 10 Seurat Clustering Mistakes Beginners Make (With Solutions)
```

**2. How-To Format (Best for featured snippets)**
```
How to [action] [data/tool] [with tool/method]
[Action] [data type] with [tool]: [benefit]

Examples:
- How to Create Seurat Objects from GEO scRNA-seq Data | Complete R Tutorial
- How to Normalize CITE-seq Data with DSB | Step-by-Step Guide
- understand 10x scRNAseq and scATAC fastqs
```

**3. Resource Compilation (Great for backlinks)**
```
[Number] [resources/tools/papers] for [task/topic]
My [opinionated/curated] selection of [resources] for [field]

Examples:
- 10 single-cell data benchmarking papers
- My opinionated selection of books/urls for bioinformatics/data science curriculum
```

**4. Comparison Posts (High conversion)**
```
[Tool A] vs [Tool B]: [What to compare]
[Concept A] and [Concept B]: Understanding the difference

Examples:
- Seurat vs Scanpy: Complete Comparison for scRNA-seq Analysis (2025)
- Understanding p value, multiple comparisons, FDR and q-value
```

**5. Deep Dive Tutorials**
```
[Exploring/Understanding] [Topic]: [Specific aspect]
[Action] [Tool/Method]: [Step/Phase]

Examples:
- Exploring Spatial Transcriptomics: A Dive into Visium Data Analysis in Python
- Calculate scATACseq TSS enrichment score
```

### Frontmatter Template

**For New .md Posts** (Recommended for best SEO control)

```yaml
---
title: "How to [Task] with [Tool] | [Benefit]"
description: "Learn to [action] using [tool]. Step-by-step [tutorial/guide] with [R/Python] code examples. [Perfect/Essential] for [audience]."
author: Ming Tang
date: '2025-01-15'
slug: keyword-rich-slug-here
categories:
  - bioinformatics
  - [single-cell/genomics/python/R]
tags:
  - [primary-keyword]
  - [tool-name]
  - [data-type]
  - tutorial
  - [methodology]
image: /img/posts_img/featured-image.png  # Optional but recommended
series: ["Tutorial Series Name"]  # Optional: for multi-part content
---
```

**Category Selection (Choose 1-2)**
- bioinformatics
- genomics
- single-cell
- R
- python
- visualization
- machine-learning

**Tag Strategy (Use 5-8 tags)**

Essential tags to include when relevant:
- scRNAseq / single-cell
- seurat
- scanpy
- DESeq2
- ATAC-seq / ChIP-seq
- tutorial
- R / python
- normalization / clustering / differential-expression
- docker / containers (for reproducibility posts)
- machine-learning / deep-learning

### Content Structure for SEO

**First 100 Words (Critical for SEO)**
- Include primary keyword in first sentence
- State the problem or use case clearly
- Mention the tools/methods you'll cover
- Set expectations for what readers will learn

Example opening:
```markdown
During my daily work with R for genomic data analysis, I encountered several
instances that R gives me some (bad) surprises. [Continues with specific examples...]
```

**Use H2 Headers with Keywords (3-5 per post)**
```markdown
## Understanding 10x single-cell RNAseq fastqs
## Are the feature barcode whitelist the same for scRNAseq and scATAC?
## Normalizing CITE-seq data: the DSB method
## Common pitfalls when clustering with Seurat
```

**Internal Linking (3-5 links per post)**
- Link to related tutorials on your blog
- Use descriptive anchor text with keywords
- Link to your resource compilations
- Connect series posts together

Example:
```markdown
Read my previous post on [understanding 10x scRNAseq fastqs](/post/understand-10x-scrnaseq/)
for more background.
```

**External Linking (2-3 authoritative sources)**
- Cite papers and methods
- Link to official documentation
- Credit other bloggers and experts
- Link to your GitHub resources

Example:
```markdown
Thanks Dave Tang for his excellent post on [10x single-cell bam files]
(https://davetang.org/muse/2018/06/06/10x-single-cell-bam-files/)
```

### Content Patterns That Rank Well

**1. Tutorial/How-To Posts**
- Problem statement
- Prerequisites/Requirements
- Step-by-step instructions with code
- Common mistakes section
- Working examples with real data
- Troubleshooting tips

**2. Gotchas/Mistakes Posts**
- List format (numbered items)
- Each gotcha gets its own section
- Explain why it's a problem
- Show the solution/fix
- Include code examples

**3. Resource Compilation Posts**
- Categorized list (Unix, R, Python, etc.)
- Brief description of each resource
- Your personal take/experience
- Call-to-action for suggestions

**4. Benchmarking/Comparison Posts**
- Introduction to the problem
- List of compared methods/tools
- Comparison table (optional)
- Links to papers/resources
- Your recommendations

**5. Deep Technical Tutorials**
- Clear introduction with motivation
- Background/theory section
- Detailed walkthrough with code
- Visualizations and examples
- Notes and caveats
- Conclusions/takeaways

### Keyword Density Guidelines

**Per 1000-word post:**
- Primary keyword: 8-12 times (0.8-1.2%)
- Secondary keywords: 5-8 times each
- Natural integration - never force it
- Include in: title, first paragraph, H2 headers, alt text, conclusion

**Example for "seurat tutorial" post:**
- ✅ "seurat" appears 10 times naturally in context
- ✅ "single cell" appears 8 times
- ✅ "clustering" appears 5 times
- ❌ "seurat seurat seurat" = keyword stuffing (avoid!)

### Featured Snippet Optimization

**Question-Answer Format**
```markdown
## What is the difference between FDR and q-value?

**FDR (False Discovery Rate)** is the expected proportion of false positives
among rejected hypotheses, while **q-value** is the minimum FDR at which a
test is called significant.

Key differences:
- **FDR**: Controls the overall rate of false discoveries
- **q-value**: The FDR threshold for each individual test
- **Example**: A q-value of 0.05 means this result is significant at FDR ≤ 5%

[Continue with detailed explanation...]
```

**List Format**
```markdown
## 5 Essential Steps for Single-Cell RNA-seq QC

### 1. Filter Low-Quality Cells

**The Problem**: Dead or damaged cells can skew your analysis...

**The Solution**: Set appropriate thresholds for...

**Code Example**:
[R/Python code here]
```

**Comparison Tables**
```markdown
## Seurat vs Scanpy: Quick Reference

| Feature | Seurat | Scanpy |
|---------|--------|--------|
| Language | R | Python |
| Speed | Moderate | Fast |
| Visualization | Excellent | Good |

**Choose Seurat if**: You work primarily in R and need publication-quality plots...
**Choose Scanpy if**: You work in Python and prioritize speed for large datasets...
```

---

## Content Topics & Opportunities

### High-Priority Topics (Target These First)

**Tier 1: High Search Volume**
- Single-cell RNA-seq tutorials (any aspect)
- Seurat workflow guides
- DESeq2 differential expression
- ATAC-seq analysis
- Data integration methods (Harmony, Seurat integration)
- Batch correction techniques

**Tier 2: Emerging Trends (Get ahead of competition)**
- Spatial transcriptomics (Visium, Xenium)
- Deep learning for genomics
- Multi-omics integration
- Cloud computing for genomics
- AI/LLMs in bioinformatics
- Single-cell multimodal (CITE-seq, ASAP-seq)

**Tier 3: Evergreen Troubleshooting**
- Common mistakes posts for any tool
- "Gotchas" in R/Python/tools
- Comparison posts (tool vs tool)
- Setup/configuration guides
- Docker/reproducibility content

### Content Gaps to Fill

**Beginner-Friendly Intros**
- "What is scRNA-seq?" (basic introduction)
- "Getting started with Seurat in 2025"
- "How to install Bioconductor packages"
- "R or Python for bioinformatics?" (you have this, optimize it!)

**Advanced Methods**
- Cell-cell communication inference
- Trajectory analysis deep dives
- RNA velocity tutorials
- Pseudobulk analysis best practices

**Workflow Posts**
- Complete end-to-end pipelines
- From GEO to publication-quality figures
- Reproducible workflows with Docker/Singularity

### Series Opportunities

Organize related posts under series taxonomy:

```yaml
series: ["Single-Cell RNA-seq Mastery"]
# Basics → QC → Normalization → Clustering → DE → Visualization

series: ["CITE-seq Complete Guide"]
# 4-part series already exists - create index page

series: ["Deep Learning for Genomics"]
# MNIST → CNNs → LSTMs → VAEs → Applications

series: ["Seurat for Single-Cell Analysis"]
# Beginner to advanced tutorials

series: ["Python for Bioinformatics"]
# Python basics → Scanpy → Advanced analysis
```

---

## SEO Checklist for New Posts

Before publishing, verify:

### Content Quality
- [ ] Title includes primary keyword (50-60 chars)
- [ ] Description is 150-160 chars with keyword and CTA
- [ ] First paragraph includes primary keyword
- [ ] 3-5 H2 headers with keywords
- [ ] 5-8 relevant tags selected
- [ ] 1-2 categories assigned
- [ ] Keyword density is 0.8-1.2% (natural, not stuffed)

### Structure & Links
- [ ] Internal links to 3-5 related posts
- [ ] External links to 2-3 authoritative sources
- [ ] Code examples are tested and working
- [ ] Images have descriptive alt text (if applicable)
- [ ] Proper heading hierarchy (H1 → H2 → H3)

### Engagement Elements
- [ ] Clear problem statement in intro
- [ ] Practical code examples included
- [ ] Personal experience or anecdote shared
- [ ] Question or call-to-action at end
- [ ] Credit given to sources/colleagues

### Technical SEO (Auto-handled)
- [ ] Slug is keyword-rich and concise
- [ ] Date is properly formatted
- [ ] Frontmatter is complete and valid
- [ ] Meta description will auto-generate (or specify custom one)
- [ ] Structured data will auto-generate from template

---

## Post-Publishing Actions

### Immediate (Week 1)
1. Share on Twitter/LinkedIn with relevant hashtags
2. Post to relevant subreddits (r/bioinformatics, r/datascience)
3. Share in Slack/Discord communities (bioinformatics, single-cell)
4. Email newsletter if you have one

### Short-term (Month 1)
1. Monitor Google Search Console for impressions
2. Check which keywords are ranking
3. Respond to comments and questions
4. Cross-link from older related posts

### Long-term (Month 3+)
1. Update with "Updated 2025" note if still relevant
2. Add new insights or tools discovered
3. Refresh screenshots/code if tools have changed
4. Add to tutorial series if applicable

---

## Writing Tips & Best Practices

### Do's ✅

**Voice & Tone**
- Write conversationally but stay technical
- Use "I" and "my" naturally - share your experience
- Be honest about challenges and limitations
- Credit sources and colleagues generously

**Content**
- Start with a clear problem or use case
- Show working code with real data
- Explain the "why" behind recommendations
- Include common mistakes and gotchas
- Reference official documentation
- Link to your GitHub resources

**SEO**
- Use keywords naturally in titles and headers
- Write descriptive meta descriptions
- Link to related posts on your blog
- Include 5-8 relevant tags
- Answer questions that appear in "People Also Ask"

**Engagement**
- Ask questions to engage readers
- Invite comments and feedback
- Include code that readers can run
- Share personal insights and lessons learned

### Don'ts ❌

**Voice & Tone**
- Don't use overly formal academic language
- Don't hide behind passive voice ("it was found that...")
- Don't oversell or use marketing hype
- Don't use emojis unless naturally appropriate

**Content**
- Don't skip the gotchas and caveats
- Don't use toy datasets when real data is available
- Don't assume readers know everything
- Don't forget to test your code examples
- Don't ignore alternative approaches

**SEO**
- Don't keyword stuff
- Don't use clickbait titles that don't deliver
- Don't forget internal links to your own content
- Don't skip the meta description (even though it auto-generates)
- Don't use very short titles (<40 chars) or very long (>70 chars)

**Formatting**
- Don't wall-of-text without breaks
- Don't forget code syntax highlighting
- Don't skip section headers
- Don't use ambiguous variable names in code

---

## Examples of Great Post Types

### Example 1: Gotchas Post
```markdown
---
title: "5 Hidden Seurat Clustering Gotchas (And How to Fix Them)"
description: "Avoid common Seurat clustering mistakes that lead to wrong results.
Learn 5 critical issues in single-cell RNA-seq clustering with R code solutions."
categories: [bioinformatics, single-cell]
tags: [seurat, scRNAseq, clustering, tutorial, R]
---

Seurat is the go-to tool for single-cell RNA-seq clustering, but there are
several gotchas that can lead you astray. During my work analyzing dozens of
scRNA-seq datasets, I've encountered these issues multiple times...

## 1. Using the Wrong Resolution Parameter

**The Problem**: Too high resolution fragments real cell types...

**The Fix**: Start with 0.4-0.8 and iterate...

**Code Example**:
[Include working R code]

[Continue for 4 more gotchas...]
```

### Example 2: How-To Tutorial
```markdown
---
title: "How to Create Seurat Objects from GEO scRNA-seq Data | Complete Guide"
description: "Step-by-step tutorial to download and process scRNA-seq data from
GEO into Seurat objects. Includes R code and troubleshooting tips."
categories: [bioinformatics, single-cell]
tags: [seurat, scRNAseq, GEO, tutorial, R, data-processing]
series: ["Seurat for Single-Cell Analysis"]
---

Want to reanalyze published scRNA-seq data from GEO? I've done this dozens of
times, and while it seems straightforward, there are several steps where things
can go wrong. Here's my complete workflow...

## Prerequisites

You'll need:
- R version 4.0+
- Seurat installed
- Basic familiarity with R

## Step 1: Download Data from GEO

First, identify your dataset. For this tutorial, I'll use GSE12345...

[Detailed steps with code...]

## Common Issues and Solutions

### Issue 1: Matrix format not recognized
**Solution**: Convert to correct format...

[Continue with complete workflow...]
```

### Example 3: Resource Compilation
```markdown
---
title: "10 Essential Papers for Understanding CITE-seq Normalization"
description: "Curated list of the best CITE-seq normalization papers with
summaries. Master antibody-derived tag normalization for multimodal single-cell."
categories: [bioinformatics, single-cell]
tags: [CITE-seq, normalization, papers, scRNAseq, multimodal]
---

CITE-seq normalization is tricky, and the field is still evolving. I've compiled
the 10 most important papers that shaped my understanding...

1. **[Paper title]** by Authors (Year)

   Key insight: [What makes this paper important]

   Link: [URL]

[Continue for all 10...]

What papers did I miss? Comment below with your recommendations!
```

---

## Measuring Success

### Key Metrics to Track

**Google Search Console (Monthly)**
- Organic impressions trend
- Average CTR (target: 3-5% for tutorials)
- Average position (target: top 10 for target keywords)
- Top performing queries
- Featured snippets captured

**Google Analytics (Monthly)**
- Pageviews per post
- Avg. time on page (target: 3-5 min for tutorials)
- Bounce rate (target: <60%)
- Traffic sources breakdown

**Engagement Metrics**
- Comments and questions
- Social shares
- GitHub stars/issues (if code repos linked)
- Newsletter signups (if applicable)

### Success Indicators

**Short-term (1-3 months)**
- Post ranks in top 20 for target keyword
- Steady increase in impressions
- Internal links drive traffic to post
- Social shares in bioinformatics communities

**Long-term (6-12 months)**
- Post ranks in top 5 for primary keyword
- Captures featured snippet
- Generates consistent organic traffic
- Gets backlinks from other sites
- Referenced in other tutorials/courses

---

## Quick Reference: SEO Title Formulas

Copy and adapt these proven formulas:

```
How to [Action] [Tool/Data] [Method] | [Benefit]
[Number] [Adjective] [Topic] [Mistakes/Tips/Tools] ([And How to Fix Them])
[Tool A] vs [Tool B]: [Comparison Aspect] for [Use Case]
Complete Guide to [Topic]: From [Start] to [End]
Understanding [Concept]: [Specific Aspect]
[Action] [Data Type] with [Tool]: [Step/Phase]
My [Curated/Opinionated] [Resource Type] for [Field/Task]
```

## Quick Reference: Keywords by Priority

**Always relevant:**
- single cell / scRNAseq / single-cell RNA-seq
- seurat / scanpy
- bioinformatics / computational biology
- R / python / tutorial

**High value when applicable:**
- DESeq2 / differential expression
- ATAC-seq / ChIP-seq / epigenomics
- normalization / clustering / QC
- batch correction / data integration
- spatial transcriptomics / visium

**Emerging opportunities:**
- deep learning / neural networks / AI
- CITE-seq / multimodal
- cloud computing / docker / reproducibility
- cell-cell communication

---

## Final Notes

**Remember the core principles:**

1. **Readers first, SEO second** - Write for humans who need solutions
2. **Share your actual experience** - Your unique insights matter
3. **Be practical and honest** - Include the gotchas and caveats
4. **Credit generously** - Link to sources, colleagues, and resources
5. **Make it actionable** - Always include working code and examples
6. **Stay authentic** - Your conversational, helpful voice is what makes this blog special

The goal isn't to game search engines - it's to create genuinely helpful content
that solves real problems for bioinformaticians. When you do that well and follow
these SEO guidelines, search rankings will follow naturally.

**Questions? Ideas for new posts?**
- Check existing posts for style inspiration
- Review SEO_KEYWORDS_REFERENCE.md for keyword ideas
- Consult SEO_IMPROVEMENTS.md for technical details

Happy writing! 🧬

---

**Document Version**: 1.0
**Last Updated**: January 2025
**Status**: Ready for use
**Maintained by**: Claude AI Assistant for @Ming Tommy Tang
