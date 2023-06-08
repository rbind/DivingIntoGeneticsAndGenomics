---
title: 'Unlock the Power of Genomics Data Analysis: Watershed''s Seamless Cloud Computing
  Solution'
author: Ming Tang
date: '2023-06-07'
slug: unlock-the-power-of-genomics-data-analysis-watershed-s-seamless-cloud-computing-solution
categories:
  - bioinformatics
  - cloud computing
  - data management
tags:
  - bioinformatics
header:
  caption: ''
  image: ''
---

**Disclaimer: This post is sponsored by [Watershed Omics Bench platform](https://www.watershed.ai/). I have personally tested the platform.**

As an experienced bioinformatician who understands the needs of biotech startups, I know the challenges that arise when analyzing genomics data. The first solution that comes to mind is cloud computing. Unsurprisingly, AWS and Google Cloud Platform (GCP) are commonly used options. Cloud computing avoids large upfront costs and can scale more effectively compared to building an on-premise solution. Setting up an on-premise solution is cumbersome, requiring meticulous organization and robust security measures.

However, setting up an AWS/GCP account for your company can be time-consuming and daunting, especially for those new to the process. It requires setting up the organization, billing account, security measures, and functioning as an administrator. The initial setup may involve multiple meetings with customer support teams. This administrative burden can be overwhelming, hindering the swift start of your analysis.

Once you've successfully set up your cloud computing account, you can spin up a virtual machine and delve into real bioinformatics work. However, there are additional challenges to navigate, such as selecting the appropriate machine types, determining optimal memory usage, configuring external disk mounting, and fine-tuning security settings. These decisions can be perplexing, particularly for those without extensive cloud computing expertise.

Take RNA-seq as an example. Running a bulk RNA-seq analysis on the cloud requires understanding and implementing workflows such as Snakemake or Nextflow. Installing the required tools can be a challenge too. Moreover, the workflow itself can be inefficient, generating large intermediate files and requiring careful management of disk space. I've found myself contending with the need to expand disk size. After overcoming these obstacles, you can obtain the count matrix and proceed to downstream differential expression analysis.  Tools like DESeq2, EdgeR, or Limma in R are commonly used, each with its own learning curve.

In stark contrast, the [Watershed Omics Bench platform](https://www.watershed.ai/) addresses these pain points and provides an intuitive and efficient solution. With Watershed, you can bid farewell to the worries of configuring CPU numbers and security settings. The platform seamlessly handles these aspects, saving you valuable time and effort. The pre-written workflows (**bulk/single-cell RNA, WGS, ATACseq, ChIPseq, and many more**) are designed to work out of the box, eliminating the need for extensive setup or workflow language comprehension. Simply load your fastq files into the system, click run, and obtain the count matrix effortlessly. Importantly, the system tracks how each file is generated, associating timestamps, commands used, and configurations as metadata. This makes the analysis highly reproducible.

Additionally, **Watershed offers real-time monitoring of your analysis job through a user-friendly interface.** This feature allows you to track the progress of your analysis, empowering you with up-to-date information and enabling informed decision-making. The platform is scalable to your needs. It allows easy parallel processing by adjusting the number of CPUs through dragging a slider. Say goodbye to the complexities of setting up Kubernetes clusters—Watershed handles it seamlessly.

Furthermore, Watershed streamlines downstream analysis by offering pre-configured DESeq2 or EdgeR modules, so you can rest assured that you are using industry best practices. These steps simplify group comparison selection. The generated figures, such as interactive volcano plots and heatmaps, facilitate data exploration. **Bioinformaticians can configure any settings at each step and even hide the underlying code in the notebook, exposing only GUI widgets to wet biologists. The GUI widgets enable them to explore the data themselves.** This feature saves time in communication and expedites analysis turnover.

In summary, the Watershed platform is an elegant solution when it comes to analyzing genomics data. It addresses the pain points associated with cloud computing by simplifying the setup process, providing a user-friendly interface with real-time monitoring, ensuring scalability, and streamlining downstream analysis. 

[Watershed](https://www.watershed.ai/) enables biotech startup hires to efficiently analyze their data, collaborate effectively, and accelerate research advancements.
