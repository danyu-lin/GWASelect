# **GWASelect**

# **GWASelect: A Variable Selection Method for Genomewide Association Studies**

For analyzing Genomewide Association Studies (GWAS) data, virtually all the existing methods focus on one SNP at a time, such as the Armitage trend test. These univariate methods do not take full advantages of genomewide data and are often lack of statistical power. Furthermore, they do not naturally lead to a model that can be readily used for disease prediction. A possible way to overcome these difficulties is to conduct variable selection at the genome-wide level. GWASelect implements a novel variable selection method for GWAS data and is able to handle more than half million SNPs. Extensive simulation studies and real data analysis show that this method enjoys high power and low false discovery rate compared to existing variable selection methods ([Bioinformatics (2010) doi: 10.1093/bioinformatics/btq600](http://bioinformatics.oxfordjournals.org/content/early/2010/10/28/bioinformatics.btq600.abstract), first published online: October 29, 2010). The variables selected by GWASelect can be readily placed into a logistic regression model for disease prediction. The current release is designed for binary outcome under the additive mode of inheritance. More developments are still underway and please check back frequently for updates.

## **General information**

The program is written in C++ language, facilitated by OpenMP. OpenMP is an application programming interface (API) that supports multi-platform shared memory multiprocessing programming. It is already included in a number of commonly used compilers, such as g++ and icc. The detail on how to obtain and install OpenMP can be found at <http://openmp.org/wp/openmp-compilers/>. We provide the executable file for x86-based Linux (64 bit), and it needs to be run under a parallel computing environment where OpenMP is supported. Parallel computing with OpenMP requires hardware that utilizes a shared-memory architecture. Please check with your system administrators to see whether your computing hardware meets this requirement. We currently provide the Linux-version for our software, and we plan to provide the software for other architectures in the near future.

## **Usage**

On Linux systems:

1.  Open a TC shell by typing: bsub -q int -Ip tcsh

2.  Typing: setenv OMP_NUMBER_THREADS 8

3.  <div>

    -   For dynamic model size, use this form of command:\

        > GWASelect -n 8 -case case_genotype_file -control ctrl_genoypte_file -exclusion bad_snp_file -dynamic stable_selection_threshold -out output_file

    -   For static model size, use this form of command:\

        > GWASelect -n 8 -case case_genotype_file -control ctrl_genoypte_file -exclusion bad_snp_file -static user_specified_model_size -out output_file

    </div>

## **Input**

In light of the gigantic volume of GWAS data, we strive to make our software as friendly as possible. The data format we require is fairly straightforward and should be very easy to follow.

This software requires 22 genotype files for controls, and 22 genotypes for cases. Since (1) we are only dealing with binary phenotype and (2) the cases and controls’ genotypes are stored in different files, there is no need to have a phenotype file. Here, the number 22 refers to the 22 autosomal chromosomes for each subject. The sex-chromosomes are not considered. Each genotype file should be a plain text file, and be named in a systematic manner such that its name ends with the corresponding chromosome number (see Example for more detail).

Assume that you have typed p SNPs for each subject, and your data have n0 controls and n1 cases. Each control-genotype file should be organized into p rows and n0 + 1 columns, and each case-genotype file into p rows and n1 + 1 columns. The p rows represent p SNPs with one SNP in each row. The columns must have the following format:

> **Column 1**: SNP ID. This column is required and is alpha-numerical. No missingness is allowed.

> **Column 2 – column (n+1)**: Genotype values. The j-th column of the i-th row pertains to the value of the i-th SNP for the (j-1)-th subject in the phenotype file. Accepted genotype values are 0, 1, and 2. GWASelect allows for very mild missingness of the genotype data, and missing values are indicated by 9. However, if there is a large amount of missing genotypes, say, greater than 1%, the user is recommended to impute the missing genotypes by other available softwares before applying GWASelect.

The user may often want to manually exclude some SNPs due to various reasons. Those to-be-excluded SNPs should be organized into one file with two columns. The first column specifies the chromosome number on which a SNP is located, and the second column specifies the SNP ID (see example below for detail).

## **Output**

This software writes computational results to the output file named by the user with the --out command-line option. This software reports SNPs that have p-values \< 1e-7 in the Armitage trend test, and SNPs that are selected by the GWASelect, along with other accessory information.

## **Example**

Assume that you have 22 genoypte files for the controls and 22 genotype file for the cases. The critical message here is that, the names of your genotype files MUST end with the corresponding chromosome numbers so that GWASelect can tell where they are from. For example, you may choose to name your control-genotype files as UNC_1, UNC_2, …, UNC_22, and your case-genotype files as diabetes_1, diabetes_2, …, diabetes_22.

UNC_22 file:

> rs2035467  0 2 1 2 9 … 2 1 2rs1222478  1 2 0 2 1 … 1 0 1
>
> …
>
> rs3400126  2 1 9 0 0 … 1 2 1

diabetes_22 file:

> rs2035467  1 0 2 2 0 … 2 0 2 2 1 2rs1222478  1 1 1 0 1 … 2 0 0 1 0 1
>
> …
>
> rs3400126  0 9 0 0 0 … 0 1 1 1 2 1

You may also happen to have dozens of SNPs that you would like to exclude from the analysis, so you organize them into one file called bad_snps.txt with two columns.

bad_snps.txt file:

> 1  rs2345121\
> 1  rs3222502\
> 2  rs3433456\
> 8  rs5004331\
> 8  rs4333892\
> …\
> 21 rs6555323\
> 21 rs7666900\
> 22 rs8233111

The final model size can be determined in two different ways. One way is to let GWASelect pick a pre-specified number of SNPs for the final model (say, 15), then the command would look like

> GWASelect -control UNC\_ -case diabetes\_ -exclusion bad_snps.txt -static 15 -output my_science_paper.txt

Another way is to let GWASelect choose a suitable model size by itself. In this case, the command would be

> GWASelect -control UNC\_ -case diabetes\_ -exclusion bad_snps.txt -dynamic 0.2 -output my_nature_paper.txt

Here, 0.2 is the stable-selection-threshold that needs to be specified by the user. We suggest the user to use a number between 0.1 and 0.2.

## **Reference**

Q. He and D.Y. Lin. A variable selection method for genome-wide association studies. ([Bioinformatics (2010) doi: 10.1093/bioinformatics/btq600](http://bioinformatics.oxfordjournals.org/content/early/2010/10/28/bioinformatics.btq600.abstract), first published online: October 29, 2010)

## **Download**

#### **GWASelect package for x86-based, 64-bit Linux \[updated: Feb 17, 2010\]**

executable file, example files, readme.txt **»** [GWASelect-1.0-linux-64-static.tar.gz](http://dlin.web.unc.edu/wp-content/uploads/sites/1568/2013/01/GWASelect-1.0-linux-64-static.tar_.gz)
