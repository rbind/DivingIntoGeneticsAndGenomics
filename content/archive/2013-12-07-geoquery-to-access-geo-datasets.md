---
title: GEOquery to access GEO datasets
author: Ming Tang
date: '2013-12-07T22:08:00-08:00'
lastmod: '2013-12-07T22:08:00-08:00'
slug: geoquery-to-access-geo-datasets
tags:
- GEO
- microarray
- R
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/12/geoquery-to-access-geo-datasets.html
description: I was reading a paper Dual functions of Tet1 in transcriptional regulation in mouse embryonic stem cells , and wanted to re-analyze the microarray data.\ In…
summary: I was reading a paper Dual functions of Tet1 in transcriptional regulation in mouse embryonic stem cells , and wanted to re-analyze the microarray data.\ In…
header:
  caption: ''
  image: ''
---

I was reading a paper [Dual functions of Tet1 in transcriptional regulation in mouse embryonic stem cells](http://www.nature.com/nature/journal/v473/n7347/full/nature09934.html) , and wanted to re-analyze the microarray data.\
In the paper, Hao wu et.al. knocked down Tet1, a protein can convert 5-methylcytosine to 5-hydroxymethlycytosine, and looked at gene expression change in mouse ES cells.

It was my first time to look at mouse microarray data set, but I was aware of that GEOquery, a bioconductor package, can do the job very easily. I will lay down the R code first.

{{< gist crazyhottommy 7853491 >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/7853491)

some output from the code:

``` r
> show(pData(phenoData(gse[[1]]))[,c(1,6,8)])
                                              title type                   source_name_ch1
GSM659775                          Control KD, rep1  RNA         control KD mouse ES cells
GSM659776                          Control KD, rep2  RNA         control KD mouse ES cells
GSM659777                          Control KD, rep3  RNA         control KD mouse ES cells
GSM659778                          Control KD, rep4  RNA         control KD mouse ES cells
GSM659779                             Tet1 KD, rep1  RNA            Tet1 KD mouse ES cells
GSM659780                             Tet1 KD, rep2  RNA            Tet1 KD mouse ES cells
GSM659781                             Tet1 KD, rep3  RNA            Tet1 KD mouse ES cells
GSM659782                             Tet1 KD, rep4  RNA            Tet1 KD mouse ES cells
GSM659783 Tet1 KD + Nanog overexpression (OE), rep1  RNA Tet1 KD + Nanog OE mouse ES cells
GSM659784 Tet1 KD + Nanog overexpression (OE), rep2  RNA Tet1 KD + Nanog OE mouse ES cells
GSM659785 Tet1 KD + Nanog overexpression (OE), rep3  RNA Tet1 KD + Nanog OE mouse ES cells
GSM659786 Tet1 KD + Nanog overexpression (OE), rep4  RNA Tet1 KD + Nanog OE mouse ES cells
```

I am only interested in gene expression change after Tet1 Knock down, so I set coef =1 in the command ( or you can specify a vector c(1,2,3) to look at comparisons among all three groups)

`topgenes<- topTable(fit2, coef=1,number=2000, p.value=0.23, lfc=0.6, adjust="BH")`

The original paper claims that there are total 1332 genes that are differentially expressed (788 upregulated and 544 downregulated in Tet1 KD cells) with FDR=0.05 ( they used another bioconductor package called NIA array analysis tool.  I am not sure what are the fold changesfor those genes). However, I only found 610 probes ( less genes, some genes have multiple probes) are differentially expressed with adjusted p.value = 0.05 ( the p.value argument in the above command is  actually the adjust p.value which is similar with the FDR) and a minimal fold change of 1.5 ( log fold change = 0.6, 2^0.6 = 1.51).

Nevertheless, I went on and sanity checked my gene list. It is a Tet1 KD experiment, so Tet1 should be downregulated.

``` r
head(topgenes)
                       ID Symbol     logFC   AveExpr         t      P.Value    adj.P.Val        B
1425532_a_at 1425532_a_at   Bin1  1.481071  7.455563  12.40969 1.996707e-08 0.0009005347 9.103139
1429448_s_at 1429448_s_at   Tet1 -2.246819 10.157653 -11.13936 6.989828e-08 0.0010641629 8.115987
1434369_a_at 1434369_a_at  Cryab  3.615875  8.073085  11.12716 7.078532e-08 0.0010641629 8.105799
1455425_at     1455425_at   Tet1 -2.145540 11.007240 -10.83405 9.615218e-08 0.0010841398 7.856887
1452253_at     1452253_at  Crim1  1.629262  5.253925  10.40753 1.520152e-07 0.0010997147 7.479479
1450780_s_at 1450780_s_at  Hmga2  2.138363  9.856595  10.36264 1.596633e-07 0.0010997147 7.438677
```

It showed up in the top list. logFC is -2.24 and -2.14 for two probes, demonstrating a really good KD. p value and adjust p value are both very small. This result reassured the correctness of my analysis.

In figure 4c, the authors used several gene examples to show Ezh2 binding is impaired after Tet1 KD, but they did not show the RT-qPCR result ( or at least need to check the microarray data). Those genes are supposed to be upregulated after Tet1 KD.

Of all the 8 examples, Lhx2, Eomes, Cdx2 did not show up in my gene list (even after I set the adjust p.value to 0.4). The others- Cyr61, Hoxa1, Sox17, Pcdh8, Gata6 are all upregulated in my list with relatively big adjust p value (0.1).

``` r
                     ID Symbol     logFC   AveExpr         t      P.Value    adj.P.Val        B
```

``` r
1457823_at     1457823_at  Cyr61 0.8698561 5.751129 3.763281 2.498490e-03 0.097058042 -1.4601779
```

``` r
1420565_at     1420565_at  Hoxa1 0.9393792   8.349 3.755187 0.002536592   0.09759434 -1.474719
```

``` r
1421657_a_at 1421657_a_at  Sox17 1.383838 6.663369 4.282790 9.577202e-04 0.057282062 -0.5375835

1447825_x_at 1447825_x_at  Pcdh8 1.388784 7.709455 3.422685 0.004741331    0.1354996 -2.074440
1417051_at     1417051_at  Pcdh8 1.480749 7.636717 3.380923 0.005131004    0.1412807 -2.149954

1425463_at 1425463_at  Gata6 0.8957893 6.507380 3.872267 0.0020389307    0.08715337 -1.2648024

It is not surprising that Tet1 maintains the expression of a group of active genes by keeping the promoters hypomethylated. Interestingly, Tet1 also maintains hypomethlyation state of bivalent gene promoters that favours PRC2, a silencing complex, to bind to theses promoters. Consequently, Tet1 contributes to the repression of Polycomb-targeted genes.

With this take home message in mind, I have several questions concerning the paper:

1. in figure 4, the authors showed several example genes that are repressed by Tet1. Are all these genes are bivalent? The authors only showed a H3k27Me3 track in figure 4b. I would at least do a ChIP-qPCR to confirm H3k4me3 and H3k27me3 occupancy at these promoters.

2. what is the exact mechanism that Tet1 represses gene expression?

The authors provided an explanation that Tet1-mediated hypomethylation is required for PRC2 silencing complex recruitment at bivalent promoters ( at least in figure 4c, the primers for Ezh2 ChIP-qPCR located at promoters). However, many shaded regions are in gene bodies. Note that the scale of the genomic regions is big. A close inspection reveals that Lhx2, Cyr61, Hoxa1, Pcdh8 gained DNA methylation in the gene bodies. It partially explains the upregulation of the gene expression after Tet1 KD (DNA methlyation in the gene body positively correlates with active gene expression). Do H3k27me3 occupancies decrease at these promoters? A ChIP-qPCR in Tet1 KD cells and control cells will answer this question.

In contrast, Eomes, Sox17 and Gata6 gained DNA methylation at promoters which may prevent the PRC2 binding and thus increase the expression. Do these genes also loss H3k27me3? It is still hard for me to believe that gain of DNA methylation at promoters can increase the gene expression. Histone modifications are more reversible, but DNA methylation is hard to reverse and is considered as a late stage of tumor suppressor silencing during tumor progression.

A quick google I found this http://epigenie.com/dna-methylation-in-gene-activation-with-dr-robert-waterland/

3'CpG island methlyation activates gene expression by interfering  enhancer blocking activity mediated by CTCF, an insulator protein that blocks enhancers from activating unintended targets. I worked on CTCF for a while, and knew  that DNA methylation abolishes CTCF binding at Igf2/H19 imprinting control region. It makes sense that 3'CpG methylation can enhancer gene expression by disrupting CTCF binding, thus allows 3' enhancers to induce the gene expression. Same rationale could be applied to 5'CpG island though. 5' enhancers are made accessible if the 5'CpG island is methylated resulting in a diminished CTCF binding at promoter region. Collectively, It is context dependent that whether DNA methylation can increase or decrease gene expression.

Apparently, we have not  fully understood the function of DNA methylation, although it is commonly thought to contribute to gene silencing. However, we often find exceptions in biology, and  the exceptions lead to good papers if you have enough evidence to support. That's the beauty of science.

3. For heatmap in figure 3c, DNA methlyation data did not plot together with the histone modification profile and gene expression profile. Ideally, the Tet1-activated targets gain DNA methlyation at promoters after Tet1 KD, but I guess the pattern did not look good (see point 2, many are gained at gene bodies). There are many reasons for that. one of the reasons is that the upregulation of these group of genes are indirect effect of Tet1 KD, although the Tet1 ChIP-seq showed binding at promoters. It will be interesting to examine where the gain of DNA methylation occur at the Tet1-activated targets and where the loss of DNA methylation occur at the Tet1-repressed targets in terms of promoters or gene bodies ( I bet the authors have done that).

Several other genes mentioned in the paper:
```

``` r
                      ID Symbol    logFC  AveExpr        t      P.Value   adj.P.Val        B
1423691_x_at 1423691_x_at   Krt8 3.163397 10.06588 8.237845 2.023202e-06 0.002343863 5.243374
```

``` r
1435989_x_at 1435989_x_at   Krt8 2.655900 10.08545 7.189101 8.477813e-06 0.004726375 3.942816
1420647_a_at 1420647_a_at   Krt8 2.956087 10.54748 7.154126 8.913416e-06 0.004902488 3.896674
```

``` r
1421657_a_at 1421657_a_at  Sox17 1.383838 6.663369 4.282790 9.577202e-04 0.057282062 -0.5375835

1429388_at 1429388_at  Nanog -1.381565 12.00124 -3.010861 0.01035011 0.2014412 -2.817704
```

``` r
1441921_x_at 1441921_x_at  Esrrb -1.00944 9.764298 -3.406862 0.004885327 0.1374205 -2.103051

1422458_at 1422458_at   Tcl1 -1.30934 10.30728 -2.86801 0.01356834 0.2250628 -3.073405

Note that Nanog and Tcl1 have relatively big adjust p value (~0.2). A  false discovery rate of 0.2 means in 100 genes, there are 20 will be false positives.

Reading this post on Biostar http://www.biostars.org/p/18470/  cleared some of my doubts on FDR choosing. The bottom line is that there is no "right" cut-off for FDR. As long as the result make biological sense, we can play around with it.

In my analysis, I set the FDR to 0.23 to include Nanog and Tcl1. I have total 1292 (1067/1332 have a Tet1 binding within 5kb up- or down-stream of TSS in the paper ) unique genes that are differentially expressed which is pretty close to the original paper. Among them, 828 (677 in the paper)  are upregulated while 464(390 in the paper) are downregulated. This is consistent with the paper: more Tet1 targets are upregulated rather than downregulated with Tet1 depletion. I will need to work on the Tet1 ChIP-seq data to determine the exact number.

tommy@tommy-ThinkPad-T420:~/Tet1$ cat  topgenes_Fdr_0.23.txt | cut -f 3 | sort | uniq | grep -v "NA" | wc -l
1292

tommy@tommy-ThinkPad-T420:~/Tet1$ cat  topgenes_Fdr_0.23.txt | grep -v "NA" > topgenes_Fdr_0.23_no_NA.txt

tommy@tommy-ThinkPad-T420:~/Tet1$ cat topgenes_Fdr_0.23_no_NA.txt | cut -f 3,4 | awk '!a[$1]++' | awk '{if($2>0) a++; else b++} END{ print a,b}'
828 464

Look at the distribution of the p value and adjust p values.

> hist(topgenes$adj.P.Val)
> hist(topgenes$P.Val)

P values are all very small.

For downstream analysis, read this post from Stephen Turner: http://gettinggeneticsdone.blogspot.com/2012/03/pathway-analysis-for-high-throughput.html

Conclusions:

1. It is a successful Tet1 KD experiment with a very good Tet1 KD and many differentially expressed genes.

2. There is no "right" way to analyze the data. Analyzing the same data by different people and different software may give different results, and it is not surprising to me. Actually, for the RNA-seq analysis, many different tools are developed ( most famous ones are cuff-diff, DESeq), and they all give different list of differentially expressed genes.  See papers here:

http://genomebiology.com/2013/14/9/R95

http://www.biomedcentral.com/1471-2105/14/91

Thus, it is critical to provide the raw code, the parameters and the software version etc to ensure a reproducible research.

3. conclusions drawn from the global analysis tend to be "exaggerated", many patterns we observe are  only true for a sub set of data sets, and if we combine all the things together, the patterns conflict each other somehow. That's why when we come down to several genes to verify and those are the best examples ( I saw many global Genomic study papers use examples with "weird" gene names).

These are my little thoughts, and I am happy to discuss with the authors.
```
