---
title: systemPipeR - NGS workflow and report generation environment 
keywords: 
last_updated: Sun Mar  6 18:04:22 2016
---
Author: Thomas Girke (thomas.girke@ucr.edu)

Last update: 06 March, 2016 

Alternative formats of this tutorial:
[`.Rmd HTML`](https://htmlpreview.github.io/?https://github.com/tgirke/systemPipeR/blob/master/vignettes/systemPipeR.html),
[`.Rmd`](https://raw.githubusercontent.com/tgirke/manuals/master/vignettes/07_systemPipeR/systemPipeR.Rmd),
[`.R`](https://raw.githubusercontent.com/tgirke/manuals/master/vignettes/07_systemPipeR/systemPipeR.R),
[`HTML Slides`](https://htmlpreview.github.io/?https://github.com/tgirke/systemPipeR/master/inst/extdata/slides/systemPipeRslides.html)

## Introduction

[_`systemPipeR`_](http://www.bioconductor.org/packages/devel/bioc/html/systemPipeR.html) provides utilities for building and running automated end-to-end analysis workflows for a wide range of next generation sequence (NGS) applications such as RNA-Seq, ChIP-Seq, VAR-Seq and Ribo-Seq (Girke , 2014). Important features include a uniform workflow interface across different NGS applications, automated report generation, and support for running both R and command-line software, such as NGS aligners or peak/variant callers, on local computers or compute clusters. The latter supports interactive job submissions and batch submissions to queuing systems of clusters. For instance, _`systemPipeR`_ can be used with most command-line aligners such as `BWA` (Li , 2013; Li et al., 2009), `TopHat2` (Kim et al., 2013) and `Bowtie2` (Langmead et al., 2012), as well as the R-based NGS aligners [_`Rsubread`_](http://www.bioconductor.org/packages/devel/bioc/html/Rsubread.html) (Liao et al., 2013) and [_`gsnap (gmapR)`_](http://www.bioconductor.org/packages/devel/bioc/html/gmapR.html) (Wu et al., 2010). Efficient handling of complex sample sets (_e.g._ FASTQ/BAM files) and experimental designs is facilitated by a well-defined sample annotation infrastructure which improves reproducibility and user-friendliness of many typical analysis workflows in the NGS area (Lawrence et al., 2013). 

Motivation and advantages of _`sytemPipeR`_ environment:

1. Facilitates design of complex NGS workflows involving multiple R/Bioconductor packages
2. Common workflow interface for different NGS applications
3. Makes NGS analysis with Bioconductor utilities more accessible to new users
4. Simplifies usage of command-line software from within R
5. Reduces complexity of using compute clusters for R and command-line software
6. Accelerates runtime of workflows via parallelzation on computer systems with mutiple CPU cores and/or multiple compute nodes
6. Automates generation of analysis reports to improve reproducibility

A central concept for designing workflows within the _`sytemPipeR`_ environment is the use of workflow management containers called _`SYSargs`_ (see Figure 1). Instances of this S4 object class are constructed by the _`systemArgs`_ function from two simple tabular files: a _`targets`_ file and a _`param`_ file. The latter is optional for workflow steps lacking command-line software. Typically, a _`SYSargs`_ instance stores all sample-level inputs as well as the paths to the corresponding outputs generated by command-line- or R-based software generating sample-level output files, such as read preprocessors (trimmed/filtered FASTQ files), aligners (SAM/BAM files), variant callers (VCF/BCF files) or peak callers (BED/WIG files). Each sample level input/outfile operation uses its own _`SYSargs`_ instance. The outpaths of _`SYSargs`_ usually define the sample inputs for the next _`SYSargs`_ instance. This connectivity is established by writing the outpaths with the _`writeTargetsout`_ function to a new _`targets`_ file that serves as input to the next _`systemArgs`_ call. Typically, the user has to provide only the initial _`targets`_ file. All downstream _`targets`_ files are generated automatically. By chaining several _`SYSargs`_ steps together one can construct complex workflows involving many sample-level input/output file operations with any combinaton of command-line or R-based software. 

![](systemPipeR_files/SystemPipeR_Workflow.png)
<div align="center">**Figure 1:** Workflow design structure of *systemPipeR* </div>

The intended way of running _`sytemPipeR`_ workflows is via _`*.Rnw`_ or _`*.Rmd`_ files, which can be executed either line-wise in interactive mode or with a single command from R or the command-line using a [_`Makefile`_](https://github.com/tgirke/systemPipeR/blob/master/inst/extdata/Makefile). This way comprehensive and reproducible analysis reports in PDF or HTML format can be generated in a fully automated manner by making use of the highly functional reporting utilities available for R. Templates for setting up custom project reports are provided as _`*.Rnw`_ files by the helper package _`systemPipeRdata`_ and in the vignettes subdirectory of _`systemPipeR`_. The corresponding PDFs of these report templates are available here: [_`systemPipeRNAseq`_](https://github.com/tgirke/systemPipeR/blob/master/vignettes/systemPipeRNAseq.pdf?raw=true), [_`systemPipeRIBOseq`_](https://github.com/tgirke/systemPipeR/blob/master/vignettes/systemPipeRIBOseq.pdf?raw=true), [_`systemPipeChIPseq`_](https://github.com/tgirke/systemPipeR/blob/master/vignettes/systemPipeChIPseq.pdf?raw=true) and [_`systemPipeVARseq`_](https://github.com/tgirke/systemPipeR/blob/master/vignettes/systemPipeVARseq.pdf?raw=true). To work with _`*.Rnw`_ or _`*.Rmd`_ files efficiently, basic knowledge of [_`Sweave`_](https://www.stat.uni-muenchen.de/~leisch/Sweave/) or [_`knitr`_](http://yihui.name/knitr/) and [_`Latex`_](http://www.latex-project.org/) or [_`R Markdown v2`_](http://rmarkdown.rstudio.com/) is required. 



