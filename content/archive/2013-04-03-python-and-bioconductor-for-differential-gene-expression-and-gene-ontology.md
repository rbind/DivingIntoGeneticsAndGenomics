---
title: python and Bioconductor for differential gene expression and gene ontology analysis
author: Ming Tang
date: '2013-04-03T09:44:00-07:00'
lastmod: '2013-04-03T09:44:00-07:00'
slug: python-and-bioconductor-for-differential-gene-expression-and-gene-ontology
tags:
- genomics
- python
- R
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2013/04/python-and-bioconductor-for.html
description: This post demonstrates performing differential expression analysis of short read sequencing data using a combination of Python and the R statistical…
summary: This post demonstrates performing differential expression analysis of short read sequencing data using a combination of Python and the R statistical…
header:
  caption: ''
  image: ''
---

## I have been looking for examples like these, finally I found them. Thx 

## [Blue Collar Bioinformatics](http://bcbio.wordpress.com/)

## 

## <http://bcbio.wordpress.com/2009/09/13/differential-expression-analysis-with-bioconductor-and-python/>

## <http://bcbio.wordpress.com/2009/10/18/gene-ontology-analysis-with-python-and-bioconductor/>

##  Differential expression analysis with Bioconductor and Python

[with 9 comments](http://bcbio.wordpress.com/2009/09/13/differential-expression-analysis-with-bioconductor-and-python/#comments)

This post demonstrates performing differential expression analysis of short read sequencing data using a combination of Python and the [R statistical language](http://www.r-project.org/). Python is used a glue language to manipulate and prepare count data from short read sequencing. R and the [Bioconductor](http://www.bioconductor.org/) package are used to perform the statistical analysis. The excellent [rpy2](http://rpy.sourceforge.net/rpy2.html)package connection Python and R.\
Scientists often need to compare expression results from multiple experimental conditions. This can be done with[microarray analysis](http://en.wikipedia.org/wiki/DNA_microarray) and the wide variety of associated [statistical tests](http://www.bioconductor.org/packages/devel/Microarray.htm), but many researchers are now utilizing short read sequencing. Experiments identify transcript prevalence through [digital gene expression](http://www.illumina.com/pages.ilmn?ID=278) under multiple conditions, and we are interested in extracting statistically meaningful differences from the results. Similarly, we may be interested in differences resulting from [short RNA discovery](http://www.illumina.com/pages.ilmn?ID=277) or other next generation sequencing applications.\
Short read differential expression analysis differs from microarray analyses as it is based on counts instead of intensity scores. The [edgeR](http://www.bioconductor.org/packages/devel/bioc/html/edgeR.html) package in Bioconductor fits count data to a statistical model considering sample to sample variation, and performs [Fisher’s exact test](http://en.wikipedia.org/wiki/Fisher's_exact_test) to provide p-values associated with changes in expression between samples.\
Here we will consider the case of a two experiment analysis. A simple [Example CSV file](http://github.com/chapmanb/bcbb/blob/master/stats/diffexp_example.csv) has counts for 5 different genes under 2 conditions and total count information for the full experiment. We read the file into a dictionary of counts keyed by each condition and the gene name:

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>1<br />
2<br />
3<br />
4<br />
5<br />
6<br />
7<br />
8<br />
9<br />
10<br />
11<br />
12<br />
13<br />
14<br />
15<br />
16<br />
17</p></td>
<td><p><code>import</code> <code>csv</code><br />
<code>import</code> <code>collections</code></p>
<p><code>def</code> <code>read_count_file(in_file):</code><br />
    <code>"""Read count information from a simple CSV file into a dictionary.</code><br />
    <code>"""</code><br />
    <code>counts</code><code>=</code> <code>collections.defaultdict(</code><code>dict</code><code>)</code><br />
    <code>with</code><code>open</code><code>(in_file) as in_handle:</code><br />
        <code>reader</code><code>=</code> <code>csv.reader(in_handle)</code><br />
        <code>header</code><code>=</code> <code>reader.</code><code>next</code><code>()</code><br />
        <code>conditions</code><code>=</code> <code>header[</code><code>1</code><code>:]</code><br />
        <code>for</code> <code>parts</code><code>in</code> <code>reader:</code><br />
            <code>region_name</code><code>=</code> <code>parts[</code><code>0</code><code>]</code><br />
            <code>region_counts</code><code>=</code> <code>[</code><code>float</code><code>(x)</code><code>for</code> <code>x</code><code>in</code> <code>parts[</code><code>1</code><code>:]]</code><br />
            <code>for</code> <code>ci, condition</code><code>in</code> <code>enumerate</code><code>(conditions):</code><br />
                <code>counts[condition][region_name]</code><code>=</code> <code>region_counts[ci]</code><br />
    <code>return</code> <code>dict</code><code>(counts)</code></p></td>
</tr>
</tbody>
</table>

The dictionary is organized into [NumPy](http://numpy.scipy.org/) matrices; NumPy is a powerful numerical package for Python which integrates smoothly with our rpy2 interface to import directly into R. Here we organize our conditions and genes and then push the count data into a matrix where the columns are conditions and the rows are genes; each item is a count value. This is returned along with associated data:

- groups — A list of experimental groups, which can be used to analyze replicates. Here, two groups with a single replicate each are defined.
- sizes — The total number of counts for each experiment. This is extracted from the “Total” row of the CSV count file, and will be used for normalization of during the statistical analysis.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>1<br />
2<br />
3<br />
4<br />
5<br />
6<br />
7<br />
8<br />
9<br />
10<br />
11<br />
12<br />
13<br />
14<br />
15<br />
16<br />
17<br />
18<br />
19<br />
20<br />
21<br />
22<br />
23<br />
24<br />
25<br />
26<br />
27</p></td>
<td><p><code>import</code> <code>numpy</code></p>
<p><code>def</code> <code>get_conditions_and_genes(work_counts):</code><br />
    <code>conditions</code><code>=</code> <code>work_counts.keys()</code><br />
    <code>conditions.sort()</code><br />
    <code>all_genes</code><code>=</code> <code>[]</code><br />
    <code>for</code> <code>c</code><code>in</code> <code>conditions:</code><br />
        <code>all_genes.extend(work_counts[c].keys())</code><br />
    <code>all_genes</code><code>=</code> <code>list</code><code>(</code><code>set</code><code>(all_genes))</code><br />
    <code>all_genes.sort()</code><br />
    <code>sizes</code><code>=</code> <code>[work_counts[c][</code><code>"Total"</code><code>]</code><code>for</code> <code>c</code><code>in</code> <code>conditions]</code><br />
    <code>all_genes.remove(</code><code>"Total"</code><code>)</code><br />
    <code>return</code> <code>conditions, all_genes, sizes</code></p>
<p><code>def</code> <code>edger_matrices(work_counts):</code><br />
    <code>conditions, all_genes, sizes</code><code>=</code> <code>get_conditions_and_genes(work_counts)</code><br />
    <code>assert</code> <code>len</code><code>(sizes)</code><code>=</code><code>=</code> <code>2</code><br />
    <code>groups</code><code>=</code> <code>[</code><code>1</code><code>,</code><code>2</code><code>]</code><br />
    <code>data</code><code>=</code> <code>[]</code><br />
    <code>final_genes</code><code>=</code> <code>[]</code><br />
    <code>for</code> <code>g</code><code>in</code> <code>all_genes:</code><br />
        <code>cur_row</code><code>=</code> <code>[</code><code>int</code><code>(work_counts[c][g])</code><code>for</code> <code>c</code><code>in</code> <code>conditions]</code><br />
        <code>if</code> <code>sum</code><code>(cur_row) &gt;</code><code>0</code><code>:</code><br />
            <code>data.append(cur_row)</code><br />
            <code>final_genes.append(g)</code><br />
    <code>return</code> <code>(numpy.array(data), numpy.array(groups), numpy.array(sizes),</code><br />
            <code>conditions, final_genes)</code></p></td>
</tr>
</tbody>
</table>

The organized data is now ready to be pushed directly into an edgeR Bioconductor analysis using the rpy2 interface. Three R functions are called: the data matrices are organized into a `DGEList` object, this object is passed to `deDGE`which does the actual digital gene expression analysis, and finally `topTags` is called to retrieve a vector of differential expression p-values. The vector is translated into a Python object from which we extract the p-values and re-organize them by the initial gene indexes. The ordered p-values are then returned.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>1<br />
2<br />
3<br />
4<br />
5<br />
6<br />
7<br />
8<br />
9<br />
10<br />
11<br />
12<br />
13<br />
14<br />
15<br />
16<br />
17<br />
18</p></td>
<td><p><code>import</code> <code>rpy2.robjects as robjects</code><br />
<code>import</code> <code>rpy2.robjects.numpy2ri</code></p>
<p><code>def</code> <code>run_edger(data, groups, sizes, genes):</code><br />
    <code>robjects.r(</code><code>'''</code><br />
        <code>library(edgeR)</code><br />
    <code>'''</code><code>)</code><br />
    <code>params</code><code>=</code> <code>{</code><code>'group'</code> <code>: groups,</code><code>'lib.size'</code> <code>: sizes}</code><br />
    <code>dgelist</code><code>=</code> <code>robjects.r.DGEList(data,</code><code>*</code><code>*</code><code>params)</code><br />
    <code>ms</code><code>=</code> <code>robjects.r.deDGE(dgelist, doPoisson</code><code>=</code><code>True</code><code>)</code><br />
    <code>tags</code><code>=</code> <code>robjects.r.topTags(ms, pair</code><code>=</code><code>groups, n</code><code>=</code><code>len</code><code>(genes))</code><br />
    <code>indexes</code><code>=</code> <code>[</code><code>int</code><code>(t)</code><code>-</code> <code>1</code> <code>for</code> <code>t</code><code>in</code> <code>tags.rownames()]</code><br />
    <code>pvals</code><code>=</code> <code>list</code><code>(tags.r[</code><code>'adj.P.Val'</code><code>][</code><code>0</code><code>])</code><br />
    <code>assert</code> <code>len</code><code>(indexes)</code><code>=</code><code>=</code> <code>len</code><code>(pvals)</code><br />
    <code>pvals_w_index</code><code>=</code> <code>zip</code><code>(indexes, pvals)</code><br />
    <code>pvals_w_index.sort()</code><br />
    <code>assert</code> <code>len</code><code>(pvals_w_index)</code><code>=</code><code>=</code> <code>len</code><code>(indexes)</code><br />
    <code>return</code> <code>[p</code><code>for</code> <code>i,p</code><code>in</code> <code>pvals_w_index]</code></p></td>
</tr>
</tbody>
</table>

The final results are written to a CSV file with our ordered genes, conditions and p-value probabilities. Each gene is associated with the initial count data and p-values, the genes are sorted by p-value with the most differentially expressed genes placed first, and the ordered information is written out:

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>1<br />
2<br />
3<br />
4<br />
5<br />
6<br />
7<br />
8<br />
9<br />
10<br />
11</p></td>
<td><p><code>def</code> <code>write_outfile(outfile, genes, conditions, work_counts, probs):</code><br />
    <code>with</code><code>open</code><code>(outfile,</code><code>"w"</code><code>) as out_handle:</code><br />
        <code>writer</code><code>=</code> <code>csv.writer(out_handle)</code><br />
        <code>writer.writerow([</code><code>"Region"</code><code>]</code><code>+</code><br />
                <code>[</code><code>"%s count"</code> <code>%</code> <code>c</code><code>for</code> <code>c</code><code>in</code> <code>conditions]</code><code>+</code> <code>[</code><code>"edgeR p-value"</code><code>])</code><br />
        <code>out_info</code><code>=</code> <code>[]</code><br />
        <code>for</code> <code>i, gene</code><code>in</code> <code>enumerate</code><code>(genes):</code><br />
            <code>counts</code><code>=</code> <code>[</code><code>int</code><code>(work_counts[c][gene])</code><code>for</code> <code>c</code><code>in</code> <code>conditions]</code><br />
            <code>out_info.append((probs[i], [gene]</code><code>+</code> <code>counts))</code><br />
        <code>out_info.sort()</code><br />
        <code>[writer.writerow(start</code><code>+</code> <code>[prob])</code><code>for</code> <code>prob, start</code><code>in</code> <code>out_info]</code></p></td>
</tr>
</tbody>
</table>

Our [example output file](http://github.com/chapmanb/bcbb/blob/master/stats/diffexp_example-diffs.csv) shows the results. The [full script](http://github.com/chapmanb/bcbb/blob/master/stats/count_diffexp.py) is available for you to use and customize for your own analyses. This example demonstrates the power of the rpy2 interface. Input and output data is happily manipulated in a familiar language such as Python, and can be seamlessly integrated with excellent statistical computation in Bioconductor and other specialized R packages.

------------------------------------------------------------------------------------

## Gene Ontology analysis with Python and Bioconductor

[with 9 comments](http://bcbio.wordpress.com/2009/10/18/gene-ontology-analysis-with-python-and-bioconductor/#comments)

Last post we discussed [extracting differentially expressed genes](http://bcbio.wordpress.com/2009/09/13/differential-expression-analysis-with-bioconductor-and-python/) from a set of short read data. The output of this analysis is a large list of genes, and the next challenge is to try and extract interesting patterns from the results. Are genes in particular families over-represented? Are there a large number of genes with a particular function? These patterns can help direct your future experiments and provide an understandable grouping for biologists examining the data.\
The [Gene Ontology (GO)](http://www.geneontology.org/) project provides a standardized set of terms describing the molecular function of genes. We will use the [topGO](http://www.bioconductor.org/packages/bioc/html/topGO.html) package from the [Bioconductor](http://www.bioconductor.org/) project to identify over-represented GO terms from a set of differentially expressed genes. Python will be used to prepare the data, utilizing [rpy2](http://rpy.sourceforge.net/rpy2.html) to call [R](http://www.r-project.org/) for the statistical analysis.\
The first step is to parse input files describing the differentially expressed genes and the mapping of gene names to GO terms. For the example we will use a [simple CSV file from our previous analysis](http://github.com/chapmanb/bcbb/blob/master/stats/diffexp_example-diffs.csv) and an [equally simple file describing the gene to GO mapping](http://github.com/chapmanb/bcbb/blob/master/stats/diffexp_example-go_map.txt). For real life work, you will need to get the GO mapping file for your organism; for instance, here is the [Arabidopsis GO mapping from TAIR](ftp://ftp.arabidopsis.org/home/tair/Ontologies/Gene_Ontology/ATH_GO_GOSLIM.txt).\
The input file of differentially expressed genes is parsed into a dictionary where the keys are gene names and the values are associated p-values:

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>1<br />
2<br />
3<br />
4<br />
5<br />
6<br />
7<br />
8<br />
9</p></td>
<td><p><code>import</code> <code>csv</code></p>
<p><code>def</code> <code>parse_input_csv(in_handle):</code><br />
    <code>reader</code><code>=</code> <code>csv.reader(in_handle)</code><br />
    <code>reader.</code><code>next</code><code>()</code><code># header</code><br />
    <code>all_genes</code><code>=</code> <code>dict</code><code>()</code><br />
    <code>for</code> <code>(gene_name, _, _, pval)</code><code>in</code> <code>reader:</code><br />
        <code>all_genes[gene_name]</code><code>=</code> <code>float</code><code>(pval)</code><br />
    <code>return</code> <code>all_genes</code></p></td>
</tr>
</tbody>
</table>

The GO mapping input file is parsed into two dictionaries: one mapping genes to GO terms for the GO analysis, and the second mapping GO terms to genes for displaying the results of the analysis:

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>1<br />
2<br />
3<br />
4<br />
5<br />
6<br />
7<br />
8<br />
9<br />
10<br />
11<br />
12<br />
13<br />
14</p></td>
<td><p><code>import</code> <code>collections</code></p>
<p><code>def</code> <code>parse_go_map_file(in_handle, genes_w_pvals):</code><br />
    <code>gene_list</code><code>=</code> <code>genes_w_pvals.keys()</code><br />
    <code>gene_to_go</code><code>=</code> <code>collections.defaultdict(</code><code>list</code><code>)</code><br />
    <code>go_to_gene</code><code>=</code> <code>collections.defaultdict(</code><code>list</code><code>)</code><br />
    <code>for</code> <code>line</code><code>in</code> <code>in_handle:</code><br />
        <code>parts</code><code>=</code> <code>line.split(</code><code>"\t"</code><code>)</code><br />
        <code>gene_id</code><code>=</code> <code>parts[</code><code>0</code><code>]</code><br />
        <code>go_id</code><code>=</code> <code>parts[</code><code>1</code><code>].strip()</code><br />
        <code>if</code> <code>gene_id</code><code>in</code> <code>gene_list:</code><br />
            <code>gene_to_go[gene_id].append(go_id)</code><br />
            <code>go_to_gene[go_id].append(gene_id)</code><br />
    <code>return</code> <code>dict</code><code>(gene_to_go),</code><code>dict</code><code>(go_to_gene)</code></p></td>
</tr>
</tbody>
</table>

Next we set up our associated R environment for GO analysis. This involves loading the topGO library and creating a function to classify genes as either differentially expressed, or not. We do this based on the p-value, classifying genes with a p-value of 0.01 or less as differentially expressed:

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>1<br />
2<br />
3<br />
4<br />
5<br />
6<br />
7<br />
8<br />
9<br />
10<br />
11</p></td>
<td><p><code>import</code> <code>rpy2.robjects as robjects</code></p>
<p><code>gene_pval</code><code>=</code> <code>1e</code><code>-</code><code>2</code><br />
<code>robjects.r(</code><code>'''</code><br />
    <code>library(topGO)</code><br />
<code>'''</code><code>)</code><br />
<code>robjects.r(</code><code>'''</code><br />
    <code>topDiffGenes = function(allScore) {</code><br />
      <code>return (allScore &lt; %s)</code><br />
    <code>}</code><br />
<code>'''</code> <code>%</code> <code>gene_pval)</code></p></td>
</tr>
</tbody>
</table>

Now it’s time to run the analysis. The paramters are defined as a dictionary which initializes a `topGOdata` object:

- ontology — the type of GO ontology to analyze; molecular function (MF) is examined here.
- annot — how the GO terms are annotated; we specify they will be identified by a mapping of genes to GO ids, passed as the gene2GO argument.
- geneSelectionFun — the function we defined above to determine if genes are differentially expressed.
- allGenes — an R named vector where the names are genes and the values are p-values; we use a utility function to convert a python dictionary to the named vector.
- gene2GO — an R named vector mapping gene names to associated GO terms.

The initialized object is run using Fisher classic analysis. Several different analysis methods and test statistics are available in the package. The resulting scores are extracted and a summary table of the results is generated:

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>1<br />
2<br />
3<br />
4<br />
5<br />
6<br />
7<br />
8<br />
9<br />
10<br />
11<br />
12<br />
13<br />
14<br />
15<br />
16<br />
17<br />
18<br />
19<br />
20<br />
21</p></td>
<td><p><code>def</code> <code>_dict_to_namedvector(init_dict):</code><br />
    <code>"""Call R to create a named vector from an input dictionary.</code><br />
    <code>"""</code><br />
    <code>return</code> <code>robjects.r.c(</code><code>*</code><code>*</code><code>init_dict)</code></p>
<p><code>go_term_type</code><code>=</code> <code>"MF"</code><br />
<code>topgo_method</code><code>=</code> <code>"classic"</code> <code># choice of classic, elim, weight</code><br />
<code>params</code><code>=</code> <code>{</code><code>"ontology"</code> <code>: go_term_type,</code><br />
          <code>"annot"</code> <code>: robjects.r[</code><code>"annFUN.gene2GO"</code><code>],</code><br />
          <code>"geneSelectionFun"</code> <code>: robjects.r[</code><code>"topDiffGenes"</code><code>],</code><br />
          <code>"allGenes"</code> <code>: _dict_to_namedvector(gene_vals),</code><br />
          <code>"gene2GO"</code> <code>: _dict_to_namedvector(gene_to_go)</code><br />
          <code>}</code><br />
<code>go_data</code><code>=</code> <code>robjects.r.new(</code><code>"topGOdata"</code><code>,</code><code>*</code><code>*</code><code>params)</code><br />
<code>results</code><code>=</code> <code>robjects.r.runTest(go_data, algorithm</code><code>=</code><code>topgo_method,</code><br />
        <code>statistic</code><code>=</code><code>"fisher"</code><code>)</code><br />
<code>scores</code><code>=</code> <code>robjects.r.score(results)</code><br />
<code>score_names</code><code>=</code> <code>scores.getnames()</code><br />
<code>num_summarize</code><code>=</code> <code>min</code><code>(</code><code>100</code><code>,</code><code>len</code><code>(score_names))</code><br />
<code>results_table</code><code>=</code> <code>robjects.r.GenTable(go_data, elimFisher</code><code>=</code><code>results,</code><br />
        <code>orderBy</code><code>=</code><code>"elimFisher"</code><code>, topNodes</code><code>=</code><code>num_summarize)</code></p></td>
</tr>
</tbody>
</table>

The scores and results table are used to generate a list of over-represented GO terms with their associated p-values and descriptions:

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>1<br />
2<br />
3<br />
4<br />
5<br />
6<br />
7<br />
8<br />
9<br />
10<br />
11<br />
12<br />
13</p></td>
<td><p><code># extract term names from the topGO summary dataframe</code><br />
<code>GO_ID_INDEX</code><code>=</code> <code>0</code><br />
<code>TERM_INDEX</code><code>=</code> <code>1</code><br />
<code>ids_to_terms</code><code>=</code> <code>dict</code><code>()</code><br />
<code>for</code> <code>index, go_id</code><code>in</code> <code>enumerate</code><code>(results_table[GO_ID_INDEX]):</code><br />
    <code>ids_to_terms[go_id]</code><code>=</code> <code>results_table[TERM_INDEX][index]</code><br />
<code>go_terms</code><code>=</code> <code>[]</code><br />
<code># convert the scores and results information info terms to return</code><br />
<code>for</code> <code>index, item</code><code>in</code> <code>enumerate</code><code>(scores):</code><br />
    <code>if</code> <code>item &lt; go_pval:</code><br />
        <code>go_id</code><code>=</code> <code>score_names[index]</code><br />
        <code>go_terms.append((item, go_id, ids_to_terms.get(go_id, "")))</code><br />
<code>go_terms.sort()</code></p></td>
</tr>
</tbody>
</table>

Finally, we print out the resulting overexpressed GO terms along with associated genes:

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>1<br />
2<br />
3<br />
4<br />
5<br />
6<br />
7<br />
8<br />
9</p></td>
<td><p><code>def</code> <code>print_go_info(go_terms, go_term_type, go_to_gene):</code><br />
    <code>for</code> <code>final_pval, go_id, go_term</code><code>in</code> <code>go_terms:</code><br />
        <code>genes</code><code>=</code> <code>[]</code><br />
        <code>for</code> <code>check_go</code><code>in</code> <code>[go_id]</code><code>+</code> <code>get_go_children(go_id, go_term_type):</code><br />
            <code>genes.extend(go_to_gene.get(check_go, []))</code><br />
        <code>genes</code><code>=</code> <code>sorted</code><code>(</code><code>list</code><code>(</code><code>set</code><code>(genes)))</code><br />
        <code>print</code> <code>"-&gt; %s (%s) : %0.4f"</code> <code>%</code> <code>(go_id, go_term, final_pval)</code><br />
        <code>for</code> <code>g</code><code>in</code> <code>genes:</code><br />
            <code>print</code> <code>g</code></p></td>
</tr>
</tbody>
</table>

One tricky part of the post-analysis processing is finding all of the genes associated with an identified GO id. The GO terminology is hierarchical, so a resulting GO identifier like `GO:0042578` in the [example results file](http://github.com/chapmanb/bcbb/blob/master/stats/diffexp_example-go_results.txt) can represent both that term and more specific terms. A nice way to illustrate this for yourself is to look at the [AmiGO browser display for the term](http://amigo.geneontology.org/cgi-bin/amigo/term-details.cgi?term=GO:0042578&session_id=2471amigo1255800166).\
The result is that genes associated with an over-represented term will come from both the identified term and more specific terms. Bioconductor has an interface to the GO database, so we can extract all of the relevant terms by moving down the GO tree collecting the more specific children, given the parent term:

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>1<br />
2<br />
3<br />
4<br />
5<br />
6<br />
7<br />
8<br />
9<br />
10<br />
11<br />
12<br />
13<br />
14<br />
15<br />
16</p></td>
<td><p><code>def</code> <code>get_go_children(go_term, go_term_type):</code><br />
    <code>robjects.r(</code><code>'''</code><br />
        <code>library(GO.db)</code><br />
    <code>'''</code><code>)</code><br />
    <code>child_map</code><code>=</code> <code>robjects.r[</code><code>"GO%sCHILDREN"</code> <code>%</code> <code>(go_term_type)]</code><br />
    <code>children</code><code>=</code> <code>[]</code><br />
    <code>to_check</code><code>=</code> <code>[go_term]</code><br />
    <code>while</code> <code>len</code><code>(to_check) &gt;</code><code>0</code><code>:</code><br />
        <code>new_children</code><code>=</code> <code>[]</code><br />
        <code>for</code> <code>check_term</code><code>in</code> <code>to_check:</code><br />
            <code>new_children.extend(</code><code>list</code><code>(robjects.r.get(check_term, child_map)))</code><br />
        <code>new_children</code><code>=</code> <code>list</code><code>(</code><code>set</code><code>([c</code><code>for</code> <code>c</code><code>in</code> <code>new_children</code><code>if</code> <code>c]))</code><br />
        <code>children.extend(new_children)</code><br />
        <code>to_check</code><code>=</code> <code>new_children</code><br />
    <code>children</code><code>=</code> <code>list</code><code>(</code><code>set</code><code>(children))</code><br />
    <code>return</code> <code>children</code></p></td>
</tr>
</tbody>
</table>

The [full script](http://github.com/chapmanb/bcbb/blob/master/stats/diffexp_go_analysis.py) pulls all of this code together into a working example. You will need to update the parsers to match your input and GO mapping file, but otherwise the code can plug directly into full genome analyses of differential expression data.
