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

grep @HWI 16S.notCombined_1.fastq | awk -F ':' '{print $10}' | sort | uniq -c
   3742 barn1_sum
   4454 barn3_sum
   1146 barn501_spr
   3826 barn502_spr
   1396 barn50_sum
   4013 barn51_spr
   4411 barn56_spr
   3753 barn58_spr
   3996 barns2_sum
   4358 gbgb11_spr
   3555 gbgb7_spr
   3853 gfgb12_spr
   4209 gfgb12_sum
   3820 gfgb1_sum
   4059 gfgb2_sum
   1291 gfgb3_spr
   3568 gfgb3_sum
   1787 gfugr1_spr
   8222 gfugr2_sum
   4911 gfugr2T_spr
   4122 gfugr3_sum
   5744 gfugrsty_spr
   4572 gfugrT1_spr
   3947 gfugrT2_spr
   3625 gfvn1_sum
   4812 gfvn2_sum
   9558 gfvn3_sum
   3586 gfvn62_spr
   3657 gfvn72_spr
   1699 gfvn7_sum
   3326 gfvn92_spr
   3789 grugr2_spr
    100 neg1_16S
    118 neg2_16S
   3090 R_1_A
   3409 R_1_B
   3268 R_1_C
   5141 R_1_D
   3951 R_1_E
   5325 R_1_F
   6301 rght10_spr
   3586 rght12_spr
   4499 rght1_sum
   3871 rght2_spr
   4668 rght3_sum
   1807 rght4_sum
```
##Summer 16S Data Analysis for ESA
1. seperate samples to individual files
```
for i in `grep @HWI /data2/marsh7/160208_miseqrun/16S/16S.extendedFrags.fastq | awk -F ':' '{print $10}' | sort | uniq`; do touch "/data2/marsh7/160208_miseqrun/16S/$i.extnd.fastq"; grep -A 3  $i /data2/marsh7/160208_miseqrun/16S/16S.extendedFrags.fastq | perl -p -e 's/--\n//' >> /data2/marsh7/160208_miseqrun/16S/$i.extnd.fastq; done &

#subsample to lowest number of reads for summer samples - 17930
for i in `grep @HWI /data2/marsh7/160208_miseqrun/16S/16S.extendedFrags.fastq | awk -F ':' '{print $10}' | sort | uniq | grep sum`; do sort -k1,1 /data2/marsh7/160208_miseqrun/16S/$i.extnd.fastq | head -n $17930 >> /data2/marsh7/160208_miseqrun/16S/$i.17930.fastq; done &

#subsample to lowest number of reads for bioreactor samples - 17930
for i in `grep @HWI /data2/marsh7/160208_miseqrun/16S/16S.extendedFrags.fastq | awk -F ':' '{print $10}' | sort | uniq | grep R_1`; do sort -k1,1 /data2/marsh7/160208_miseqrun/16S/$i.extnd.fastq | head -n $17930 >> /data2/marsh7/160208_miseqrun/16S/$i.17930.fastq; done &

#create qiime compatible fasta file
```
