---
title: get an idea of a gene expression value across samples by GEOquery
author: Ming Tang
date: '2014-07-04T13:09:00-07:00'
lastmod: '2014-07-04T13:09:00-07:00'
slug: get-an-idea-of-a-gene-expression-value-across-samples-by-geoquery
tags:
- GEO
- R
math: false
blackfriday:
  smartypants: false
  fractions: false
blogspot_url: https://crazyhottommy.blogspot.com/2014/07/get-idea-of-gene-expression-value.html
description: My next-door PI wanted to look at the TRIM29 expression levels in a series of tumor xenografts from a microarray data. I used GEOquery bioconductor package…
summary: My next-door PI wanted to look at the TRIM29 expression levels in a series of tumor xenografts from a microarray data. I used GEOquery bioconductor package…
header:
  caption: ''
  image: ''
---

My next-door PI wanted to look at the TRIM29 expression levels in a series of tumor xenografts from a microarray data. I used GEOquery bioconductor package to get the log2 transformed values and plot a boxplot for him. see the code below. Also, I had a previous post on GEOquery [GEOquery to access GEO datasets](/archive/geoquery-to-access-geo-datasets/)

{{< gist crazyhottommy bd186f68af5ba3bab9b6 >}}

[View this gist on GitHub](https://gist.github.com/crazyhottommy/bd186f68af5ba3bab9b6)

after got the Expression set object, I looked the names of the object:\
`> names(pData(gset))`\
` [1] "title" "geo_accession" "status" "submission_date"`\
` [5] "last_update_date" "type" "channel_count" "source_name_ch1"`\
` [9] "organism_ch1" "characteristics_ch1" "biomaterial_provider_ch1" "molecule_ch1"`\
`[13] "extract_protocol_ch1" "label_ch1" "label_protocol_ch1" "taxid_ch1"`\
`[17] "source_name_ch2" "organism_ch2" "characteristics_ch2" "biomaterial_provider_ch2"`\
`[21] "molecule_ch2" "extract_protocol_ch2" "label_ch2" "label_protocol_ch2"`\
`[25] "taxid_ch2" "hyb_protocol" "scan_protocol" "description"`\
`[29] "data_processing" "platform_id" "contact_name" "contact_email"`\
`[33] "contact_phone" "contact_department" "contact_institute" "contact_address"`\
`[37] "contact_city" "contact_state" "contact_zip/postal_code" "contact_country"`\
`[41] "supplementary_file" "data_row_count"`

then, only look at the meta data:\
`> pData(gset)[,c(1,2,28)]`\
` title geo_accession description`\
`GSM847887 ML-5998-TG1 GSM847887 Xenograft`\
`GSM847888 Baylor 2147 TG6 GSM847888 Xenograft`\
`GSM847890 Baylor 2665A TG6 GSM847890 Xenograft`\
`GSM847891 Baylor 3107 TG5 GSM847891 Xenograft`\
`GSM847892 Baylor 3143 TG5 GSM847892 Xenograft`\
`GSM847893 Baylor 3204 TG5 GSM847893 Xenograft`\
`GSM847894 Baylor 3561 TG5 GSM847894 Xenograft`\
`GSM847895 Baylor 3611 TG5 GSM847895 Xenograft`\
`GSM847896 Baylor 3613A TG5 GSM847896 Xenograft`\
`GSM847898 Baylor 3807 TG5 GSM847898 Xenograft`\
`GSM847901 Baylor 3887 TG5 GSM847901 Xenograft`\
`GSM847902 Baylor 3904 TG5 GSM847902 Xenograft`\
`GSM847903 Baylor 3936 TG5 GSM847903 Xenograft`\
`GSM847904 Baylor 3963 TG5 GSM847904 Xenograft`\
`GSM847905 Baylor 4013 TG1 GSM847905 Xenograft`\
`GSM847907 Baylor 4175 TG1 GSM847907 Xenograft`\
`GSM847908 Baylor 4195 TG4 GSM847908 Xenograft`\
`GSM847909 Baylor 4272 TG1 GSM847909 Xenograft`\
`GSM847911 Baylor 4664 TG1 GSM847911 Xenograft`\
`GSM847914 Baylor 4888 TG1 GSM847914 Xenograft`\
`GSM847915 Baylor 4913 TG1 GSM847915 Xenograft`\
`GSM847917 ML-4189-TG2 GSM847917 Xenograft`\
`GSM847919 ML-5097-TG2 GSM847919 Xenograft`\
`GSM847920 ML-5156-TG2 GSM847920 Xenograft`\
`GSM847921 ML-5471-TG2 GSM847921 Xenograft`\
`GSM847922 9830-000060B NEW PROTOCOL V5 GSM847922 Tumor sample`\
`GSM847923 9830-000094B-244K GSM847923 Tumor sample`\
`GSM847924 9830-000424B-244Kv5 GSM847924 Tumor sample`\
`GSM847925 9830-000517B-244Kv5 GSM847925 Tumor sample`\
`GSM848100 9830-010118B NEW PROTOCOL V5 GSM848100 Tumor sample`\
`GSM848101 9830-010130B NEW PROTOCOL V5 GSM848101 Tumor sample`\
`GSM848102 9830-010214B NEW PROTOCOL V5 GSM848102 Tumor sample`\
`GSM848103 9830-010255B NEW PROTOCOL V5 GSM848103 Tumor sample`\
`GSM848104 9830-010384B NEW PROTOCOL V5 GSM848104 Tumor sample`\
`GSM848105 9830-010461B-244Kv5 GSM848105 Tumor sample`\
`GSM848106 9830-020018B-244K2008 GSM848106 Tumor sample`\
`GSM848107 9830-020025B-244Kv5 GSM848107 Tumor sample`\
`GSM848108 9830-020039B-244Kv5 GSM848108 Tumor sample`\
`GSM848109 9830-020185B-244K2008 GSM848109 Tumor sample`\
`GSM848110 9830-020310B-244Kv5 GSM848110 Tumor sample`\
`GSM848111 9830-020340B NEW PROTOCOL V5 GSM848111 Tumor sample`\
`GSM848112 9830-020416B NEW PROTOCOL V5 GSM848112 Tumor sample`\
`GSM848113 9830-030267B-244K2008 GSM848113 Tumor sample`\
`GSM848114 9830-030446B-244Kv5 GSM848114 Tumor sample`\
`GSM848115 9830-030597B-244K2008 GSM848115 Tumor sample`\
`GSM848116 UNC-000279B-244K2008 GSM848116 Tumor sample`\
`GSM848117 UNC-010208B-244K2008 GSM848117 Tumor sample`\
`GSM848118 UNC-010224B-244Kv5 GSM848118 Tumor sample`\
`GSM848119 UNC-010304B-244Kv5 GSM848119 Tumor sample`\
`GSM848120 UNC-010509B-244K2008 GSM848120 Tumor sample`\
`GSM848121 UNC-020155B-244Kv5 GSM848121 Tumor sample`\
`GSM848122 UNC-020320B-244K GSM848122 Tumor sample`\
`GSM848123 UNC-020578B-244k-DGE GSM848123 Tumor sample`\
`GSM848124 UNC-030065B-244K2008 GSM848124 Tumor sample`\
`GSM848125 UNC-030183B-244K2008 GSM848125 Tumor sample`\
`GSM848126 UNC-030370B-244K2008 GSM848126 Tumor sample`\
`GSM848127 UNC-030528B-244K2008 GSM848127 Tumor sample`\
`GSM848128 UNC-040011B-244K2008 GSM848128 Tumor sample`\
`GSM848129 UNC-960028B-244Kv5 GSM848129 Tumor sample`\
`GSM848130 WashU-15720-244Kv5 GSM848130 Tumor sample`

there are 10 probes for TRIM29 gene:\
`> gpl_ann[gpl_ann$"Blast Gene Symbol"=="TRIM29",]`\
` ID GB_ACC SPOT_ID Public id Probe NAME Blast Gene ID Blast Refseq ID Blast Gene Symbol`\
`35972 35972 NM_012101 NM_012101 NM_012101_2_2683 23650 NM_012101.3 TRIM29`\
`39854 39854 NM_012101 NM_012101 A_23_P340123 23650 NM_012101.3 TRIM29`\
`76447 76447 NM_012101 NM_012101 A_23_P340123 23650 NM_012101.3 TRIM29`\
`94361 94361 NM_012101 NM_012101 A_23_P203267 23650 NM_012101.3 TRIM29`\
`97602 97602 NM_012101 NM_012101 NM_012101_2_2608 23650 NM_012101.3 TRIM29`\
`111151 111151 NM_012101 NM_012101 NM_012101_2_2683 23650 NM_012101.3 TRIM29`\
`120477 120477 NM_012101 NM_012101 A_23_P203260 23650 NM_012101.3 TRIM29`\
`132274 132274 NM_012101 NM_012101 A_23_P203260 23650 NM_012101.3 TRIM29`\
`187541 187541 NM_012101 NM_012101 NM_012101_2_2608 23650 NM_012101.3 TRIM29`\
`190531 190531 NM_012101 NM_012101 A_23_P203267 23650 NM_012101.3 TRIM29`\
` Blast Gene Description Blast Chromosome Map Location`\
`35972 tripartite motif-containing 29 11q22-q23`\
`39854 tripartite motif-containing 29 11q22-q23`\
`76447 tripartite motif-containing 29 11q22-q23`\
`94361 tripartite motif-containing 29 11q22-q23`\
`97602 tripartite motif-containing 29 11q22-q23`\
`111151 tripartite motif-containing 29 11q22-q23`\
`120477 tripartite motif-containing 29 11q22-q23`\
`132274 tripartite motif-containing 29 11q22-q23`\
`187541 tripartite motif-containing 29 11q22-q23`\
`190531 tripartite motif-containing 29 11q22-q23`

the boxplot of the TRIM29 expression levels across different samples:

![](/img/archive/get-an-idea-of-a-gene-expression-value-across-samples-by-geoquery/01-trim29.png)
