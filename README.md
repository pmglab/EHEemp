## Scripts capable of repeating the empirical analyses of Fig. 4 in Miao *et al*. 2023

We created these scripts to describe how to repeat the empirical analyses of **Figure 4** in our published paper:

Lin Miao, Lin Jiang, Bin Tang, Pak Chung Sham, Miaoxin Li.
Dissecting the high-resolution genetic architecture of complex phenotypes by accurately
estimating gene-based conditional heritability.
The American Journal of Human Genetics (2023). https://doi.org/10.1016/j.ajhg.2023.08.006

The three methods compared in **Figure 4** are EHE (implemented by KGGSEE), HESS, and GBAT (implemented by LDAK). All
methods need the three types of input: (1) GWAS summary statistics; here includes minimized GWAS results
for Height and College and URLs for all the 42 phenotypes; (2) genotypes of a reference panel; here includes
a minimized 1KG EUR panel covering only coding genes; (3) gene regions; this repository includes a file of 
protein-coding gene regions. The following table lists files in this repository.

| File in this repository                           | Description                                                                                                                                                                                                              |
|---------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `metadata/phenotype.tsv`                          | Information of the 42 phenotypes in **Figure 4** with URLs provided by the [Neale Lab](http://www.nealelab.is/uk-biobank).                                                                                               |
| `metadata/auto_coding_with10kb_flk.vcf.gz.*`[^1]  | A minimized 1KG EUR panel containing only autosomal coding genes including 10 kb flanking regions produced from the [original 1000 Genomes VCF files](https://ftp-trace.ncbi.nih.gov/1000genomes/ftp/release/20130502/). |
| `metadata/auto_coding_with10kb_flk.region.kggsee` | The gene region files for the three methods; only autosomal protein-coding genes with 10 kb flanking regions are included.                                                                                               |
| `test/height.minimized.tsv.gz`[^2]                | A minimized GWAS result file for the test run of a quantitative phenotype                                                                                                                                                |
| `test/college.minimized.tsv.gz`[^3]               | A minimized GWAS result file for the test run of a dichotomous phenotype                                                                                                                                                 |
| `format_sumstat.py`                               | A script to format a GWAS result file downloaded from the Neale Lab to the three input formats                                                                                                                           |
| `format_region.py`                                | A script to format the region file to HESS and LDAK formats                                                                                                                                                              |
| `collect_results.py`                              | A script to collect all heritability estimates into one file and convert the estimate of a dichotomous phenotype into liability-scale                                                                                    |
| `test.sh`                                         | A script to estimate gene-based heritability for Height (**Figure 4B**) and College (**Figure 4C**)                                                                                                                      |

[^1]: Only variants satisfy all following conditions are retained: (1) be a biallelic SNP in the UKB sample, (2)
with UKB INFO score >0.9, (3) with MAF >0.01 in the 1KG EUR panel (4) with a valid dbSNP build 151 rsID, (5) contained
in all the 42 phenotypes' GWAS results, and (6) several QC conditions, e.g., `--geno 0.05 --hwe 1e-5` in PLINK. Run 
`cat metadata/auto_coding_with10kb_flk.vcf.gz.* >metadata/auto_coding_with10kb_flk.vcf.gz` to get the gzipped VCF file.

[^2]: The gene heritability estimates by HESS are obtained by decomposing the heritability estimates of all protein-coding 
genes by LDER, which are saved in the columns `LDER_cdgn_h2` and `LDER_cdgn_h2_se` of `metadata/phenotype.tsv`.

[^3]: For a dichotomous phenotype, we calculate liability-scale heritability for EHE and GBAT with the prevalence
being the proportion of affected individuals in the UKB sample. The liability-scale heritability for HESS is obtained by
decomposing the liability-scale heritability of all protein-coding genes calculated by LDER.

The project URLs of the programs needed by this repeating analysis are shown in the following table.

| Method | Project URL                         | The version we used      |
|--------|-------------------------------------|--------------------------|
| KGGSEE | http://pmglab.top/kggsee/#/         | v1.1                     |
| PLINK  | https://www.cog-genomics.org/plink/ | v1.90b6.12 (28 Oct 2019) |
| HESS   | https://huwenboshi.github.io/hess/  | v0.5 (9/October/2017)    |
| LDAK   | https://dougspeed.com/              | v5.2                     |

