# VCF2SM
Python script that integrates sequencing depth information of polymorphisms in variant call format (VCF) files and SuperMASSA software for quantitative genotype calling in polyploids.

## Dependencies

Besides [Python](https://www.python.org/) (v.2.7+) and [Matplotlib](https://matplotlib.org/), you will need the SuperMASSA source code, which is available [here](https://bitbucket.org/orserang/supermassa). SuperMASSA implements a Bayesian network to address the dosage calling problem taking genetic models into consideration, such as full-sib family and Hardy-Weinberg Equilibrium expected frequencies. Please see the original [paper](http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0030906) for details.

## Usage

```
[-h] -i INPUT -o OUTPUT
[-g GENO_PATTERN [GENO_PATTERN ...] | -r GENO_RANGE [GENO_RANGE ...]] 
[-1 [PAR1_PATTERN [PAR1_PATTERN ...]] | -k [PAR1_RANGE [PAR1_RANGE ...]] 
[-2 [PAR2_PATTERN [PAR2_PATTERN ...]] | -l [PAR1_RANGE [PAR1_RANGE ...]] 
[--sC SC] [--eC EC] [-a ALLELE_DEPTH] [-d MINIMUM_DEPTH] [-D MAXIMUM_DEPTH] 
[-S SMSCRIPT] [-I INFERENCE] [-e] [-M PLOIDY_RANGE] [-V SIGMA_RANGE]
[-f PLOIDY_FILTER] [-p POST] [-n NAIVE_REPORTING] [-c CALLRATE] [-t CORES]
```

## Input

VCF file with exact allele depth counts generated by software such as [Tassel4-Poly](https://github.com/gramarga/tassel4-poly), [FreeBayes](https://github.com/ekg/freebayes) and [GATK](https://software.broadinstitute.org/gatk/).

## Output

VCF file with quantitative genotype calls, i.e., depicting reference and alternative allele dosages.

## Example command

Data from [this paper](http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0062355) is available [here](http://journals.plos.org/plosone/article/file?type=supplementary&id=info:doi/10.1371/journal.pone.0062355.s007). You can download it and run it straight from your terminal as follows (no compilation needed):

```
python VCF2SM.py -i NewPlusOldCalls.headed.vcf -o NewPlusOldCalls.headed_new.vcf -d 1260 -D 42000 -a RA/AA -r 1:84 -S ./src/SuperMASSA.py -I hw -M 4:6 -f 4 -p 0.80 -n 0.90 -c 0.75 -t 16
```

## Arguments

|Short|Long|Description|Details
|--- | --- | --- | ---
|`-i`|`--input`|Path to VCF input file|
|`-o`|`--output`|Path to VCF output file|
|`-g`|`--geno_pattern`|Pattern(s) of genotype IDs to include as samples|
|`-r`|`--geno_range`|Indices of genotypes to includeas samples|List or range in the form `1:150`
|`-1`|`--par1_pattern`|Pattern(s) of genotype IDs for parent 1|
|`-k`|`--par1_range`|Indices of genotypes to include as parent 1|List or range in the form `1:5`|
|`-2`|`--par2_pattern`|Pattern(s) of genotype IDs for parent 2|
|`-l`|`--par2_range`|Indices of genotypes to include as parent 2|List or range in the form `1:5`|
||`--sC`|Starting chromosome|
||`--eC`|Ending chromosome|
|`-a`|`--allele_depth`|VCF field(s) to get allele depths from|Default = `AD`. It also supports `RA/AA` format
|`-d`|`--minimum_depth`|Minimum average depth per sample for variant site to be processed (not including parents, for F1 progenies)|Default = `0`
|`-D`|`--maximum_depth`|Maximum average depth per sample for variant site to be processed (not including parents, for F1 progenies)|Default = `inf`
|`-S`|`--SMscript`|Path to the SuperMASSA script|
|`-I`|`--inference`|Inference mode for SuperMASSA|`f1` or `hw` for respective F1 segregant or Hardy-Weinberg models; `ploidy` for non-model based ploidy estimation
|`-e`|`--exact`|Exact inference|Default is `FALSE` (performs approximate inference)
|`-M`|`--ploidy_range`|Ploidy range to test with SuperMASSA|Default = `2:16`
|`-V`|`--sigma_range`|Sigma range for SuperMASSA|Default = `0.01:1:0.05` (range in the form low:high:step)
|`-f`|`--ploidy_filter`|Range of ploidies to include in the output|Default = `2:16`
|`-p`|`--post`|Minimum posterior probability to keep variant|Default = `0.0`
|`-n`|`--naive_reporting`|Naive posterior reporting threshold|Default = `0.0`
|`-c`|`--callrate`|Minimum call rate to keep variant|Default = `0.0`
|`-t`|`--threads`|Maximum number of threads to use|Default = `1`

## Cite

Pereira GS, Garcia AAF, Margarido GRA. (2017) A fully automated pipeline for quantitative genotype calling from next generation sequencing data in polyploids. *BMC Bioinformatics* (submitted).
