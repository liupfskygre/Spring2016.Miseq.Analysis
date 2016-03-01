# Spring2016.Miseq.Analysis
Keeping track of data analysis steps and scripts for stream woody debris and biofilter MiSeq run

Reads were recieved from the University of Idaho demultiplexed and seperated by barcode (16S and ITS). Analysis was done on IMET server. 

```
grep -c @HWI 16S_R1.fastq 
1935582

grep -c @HWI ITS_R1.fastq 
1755113
```

1. Use FLASH (https://ccb.jhu.edu/software/FLASH/) to merge paired-end reads. M parameter (sets maximum overlap) increased (default = 65) because a large number of reads overlapped by more than 65 bases. 

```
~/packages/FLASH-1.2.11/./flash 16S_R1.fastq 16S_R2.fastq -o 16S -M 200 2>&1 > flash16S.out &
~/packages/FLASH-1.2.11/./flash ITS_R1.fastq ITS_R2.fastq -o ITS -M 90 2>&1 > flashITS.out &

grep -c @HWI 16S.extendedFrags.fastq 
1755997
grep -c @HWI 16S.notCombined_1.fastq 
179585
grep -c @HWI ITS.extendedFrags.fastq 
352737
grep -c @HWI ITS.notCombined_1.fastq 
1402376
```
