## FASTQ Trimming Tutorial  

### Why do we trim fastq files?

Adapter sequences should be removed from reads because they interfere with downstream analyses, such as alignment of reads to a reference. For example, if we don\`t remove any adaptors in an expression analysis we will not be obtaining the gene expression data because we can\`t align sequences to the genes.

![illumina_respresntative](https://supportassets.illumina.com/content/dam/illumina-support/images/bulletins/PEcell1.png 'MiSeq, HiSeq 2000/2500 and NovaSeq paired-end flow cell' )  
**Figure 1: Representative model of how adapters are used during sequencing.**

The other reason why we trim sequences can be low quality. This indicates us we should do FASTQC analysis before performing trimming. 

![bad quailty fastq](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc.png)  
**Figure 2: An example of bad sequence quality score in FASTQC analysis.**

### How adapter trimming works?
Conceptually, the procedure is as follows:

1. Consider all possible overlaps between the two sequences and compute an alignment for each, minimizing the total number of errors in each one.
2. Keep only those alignments that do not exceed the specified maximum error rate.
3. Then, keep only those alignments that have a maximal number of matches (that is, there is no alignment with more matches).
4. If there are multiple alignments with the same number of matches, then keep only those that have the smallest error rate.
5. If there are still multiple candidates left, choose the alignment that starts at the leftmost position within the read.  

### How quailty trimming works?
Assume you use a threshold of 10 and have these quality values:

42, 40, 26, 27, 8, 7, 11, 4, 2, 3

Subtracting the threshold gives:

32, 30, 16, 17, -2, -3, 1, -6, -8, -7

Then sum up the numbers, starting from the end (partial sums). Stop early if the sum is greater than zero:

(70), (38), 8, -8, -25, -23, -20, -21, -15, -7

The numbers in parentheses are not computed (because 8 is greater than zero), but shown here for completeness. The position of the minimum (-25) is used as the trimming position. Therefore, the read is trimmed to the first four bases, which have quality values 42, 40, 26, 27.

### Command Line  

Usage:  
`cutadapt -a ADAPTER [options] [-o output.fastq] input.fastq`

For paired-end reads:  
`cutadapt -a ADAPT1 -A ADAPT2 [options] -o out1.fastq -p out2.fastq in1.fastq in2.fastq`

#### Important Features


Finding adapters:
  Parameters -a, -g, -b specify adapters to be removed from each read (or
  from the first read in a pair if data is paired). If specified multiple
  times, only the best matching adapter is trimmed (but see the --times
  option). When the special notation 'file:FILE' is used, adapter sequences
  are read from the given FASTA file.

  **-a ADAPTER, --adapter ADAPTER**  
						Sequence of an adapter ligated to the 3' end.

  **-g ADAPTER, --front ADAPTER**  
                        Sequence of an adapter ligated to the 5' end.

  **-b ADAPTER, --anywhere ADAPTER**
                        Sequence of an adapter that may be ligated to the 5'
                        or 3' end .

  **--no-indels**  
  Allow only mismatches in alignments. Default: allow both mismatches and indels
						
  **-q [5'CUTOFF,]3'CUTOFF, --quality-cutoff [5'CUTOFF,]3'CUTOFF**  
                        Trim low-quality bases from 5' and/or 3' ends of each
                        read before adapter removal. Applied to both reads if
                        data is paired. If one value is given, only the 3' end
                        is trimmed. If two comma-separated cutoffs are given,
                        the 5' end is trimmed with the first cutoff, the 3'
                        end with the second.

  **-o FILE, --output FILE**

Paired-end options:  
  The -A/-G/-B/-U options work like their -a/-b/-g/-u counterparts, but are  
  applied to the second read in each pair.

  **-A ADAPTER**            3' adapter to be removed from second read in a pair.  
  **-G ADAPTER**            5' adapter to be removed from second read in a pair.  
  **-B ADAPTER**            5'/3 adapter to be removed from second read in a pair.  
  **-p FILE --paired-output FILE**	
                        

Single end command line example

``cutadapt -a AACCGGTT input.fastq > output.fastq``

Paired end command line example

``cutadapt -a ADAPTER_FWD -A ADAPTER_REV -o out.1.fastq -p out.2.fastq reads.1.fastq reads.2.fastq``

### Today\`s Tutorial
Two different data:  
[Single end data](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSM4114778)  
Adapter: NNNNNNCACTCGGGCACCAAGGAC

[Paired end data](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSM4114785)  
Adapter: AGATCGGAAGAGCACACGTCTGAACTCCAGTCAC, AGATCGGAAGAGCGTCGTGTAGGGAAAGAGTGTA

To obtain data:   
 `cp -r /cta/users/bselcuk/gwsta/trimming_tutorial/ trimming_tutorial`

**First Part: Adapter Trimming**  
1. Obtain the data end template sh file.  
2. Fill the missing parts in command line of the cutadapt.sh file.
3. Submit your sh file and state the results from your out file.

**Second Part: Remove Adapter Containing Sequences**  
1. Go to the cutadapt user guide or type cutadapt -h in order to see how you can do that.
2. Modify the sh file.
3. Submit job and state the results.

**Third Part: Remove Adapters Given In A Text File**  
1. Go to the cutadapt user guide and perform the first part by using "adapters.fa" in your directory.

### Useful Links  
Cutadapt User Guide:  
https://cutadapt.readthedocs.io/en/stable/guide.html

Illumina Adapter Sequences:  
https://support.illumina.com/downloads/illumina-adapter-sequences-document-1000000002694.html


