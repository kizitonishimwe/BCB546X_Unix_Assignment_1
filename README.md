﻿﻿﻿﻿﻿﻿﻿﻿﻿﻿﻿﻿﻿﻿﻿﻿﻿﻿﻿﻿﻿# BCB546X_Unix_Assignment
## **Data Inspection**

###1. File Size 

a) ` fang_et_al_genotypes.txt `

The file size is **11M**

- **Syntax**:  ` du -h fang_et_gentoypes.txt `
#

b) `snp_position.txt`

- **Syntax**: `du -h snp_position.txt`
##

###2. Number of lines and columns 

a) ` fang_et_al_genotypes.txt `

The file contains 2,783 lines, 2,744,038 words, and  11,051,939 characters. 

- **Syntax**:  ` cat fang_et_al_genotypes.txt | wc `

The file contains 986 columns.

- **Syntax**:  ` awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt`

##

b) `snp_position.txt`
The file contains 984 lines, 13,198 words, and 82,763 characters.

- **Syntax**: `cat snp_position.txt | wc`

The file contains 15 columns. 

- **Syntax**: `awk -F "\t" '{print NF; exit}' snp_position.txt`
#
###2. Head and Tail 

a) ` fang_et_al_genotypes.txt `

The 3 first columns represent the `Sample_ID` , `JG_OTU`, and `Group`

**Syntax**: `cat fang_et_al_genotypes.txt | head | cut -f 1,2,3` and `cat fang_et_al_genotypes.txt | tail | cut -f 1,2,3`
#
b) `snp_position.txt`

The first, the second and third columns contain SNP_ID, Chromosome, and Position respectively. 


**Syntax**: `cat snp_position.txt | head | cut -f 1,3,4` 
#

#**Data Processing**:

 **1.Grouping by genotype**: 


-Maize = ZMMIL, ZMMLR, and ZMMMR
     
**Syntax**: `grep -E "(ZMMIL|ZMMLR|ZMMMR|Group)" fang_et_al_genotypes.txt >  maize_genotypes.txt`


- Teosinte = ZMPBA, ZMPIL, and ZMPJA

**Syntax**: `grep -E "(ZMPBA|ZMPIL|ZMPJA|Group)" fang_et_al_genotypes.txt > teosinte_genotypes.txt`
#


**2. Transposing files**: 

**a) Maize Genotype**: 

**Syntax**: ` awk -f transpose.awk maize_genotypes.txt > transposed_maize_genotypes.txt`
#
**b) Teosinte Genotype**: 

**Syntax**: `awk -f transpose.awk teosinte_genotypes.txt > transposed_teosinte_genotypes.txt`

#

**3. Sorting the created files**: 

**a) Maize Genotype**: 

- Sorting the maize genotype after removing the header:

**Syntax**: `echo "$(tail -n +2 transposed_maize_genotypes.txt)" | sort -k1,1 -k3,3n -k4,4n > sorted_transposed_maize_genotypes.txt`

Check if the sorted file is well sorted: 

**Syntax**:    `echo $?` 

The result is `0` which means that the created file is well correctly sorted. 
#



**b) Teosinte Genotype**: 

- Sorting the maize genotype after removing the header:
**Syntax**: ` echo "$(tail -n +2 transposed_teosinte_genotypes.txt)" | sort -k1,1 -k3,3n -k4,4n > sorted_transposed_teosinte_genotypes.txt`

Check if the sorted file is well sorted: 

**Syntax**:  `echo $?`

The result is `0`. This result showed that this file is not sorted.
#

**c) SNP Position** : 

Sorting the file after removing the header and create another sorted file

**Syntax**: `echo "$(tail -n +2 snp_position.txt)" | sort -k1,1 -k3,3n -k4,4n > sorted_snp_position.txt`

Check if the sorted file is well sorted: 

**Syntax**: `sort -k1,1 -k3,3n -k4,4n  -c sorted_snp_position.txt`

The result is `0` which means that the created file is well correctly sorted. 
#


**4. Joining**: 

#**- Maize genotype**: 

**-10 files (1 for each chromosome) with SNPs ordered based on increasing position values and with missing
data encoded by this symbol: ?**
#

- Chromosome 1: 
First, group sort the chromosome 1 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position

**Syntax**: `awk '$3 ==1' sorted_snp_position.txt | cut -f 1,3,4 > chrom_1_sorted_snp_position.txt`


SNPs with missing data encoded by the symbol: ? 

**Syntax**: `grep '?' transposed_maize_genotypes.txt > transposed_?_maize_genotype.txt`


Join the the two files with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ? for the chromosome 1: 

**Syntax**: `join -1 1 -2 1 chrom_1_sorted_snp_position.txt transposed_?_maize_genotype.txt > chrom_1_?_incr_posit_maize_genotype.txt`
#

- Chromosome 2: 
First, group sort the chromosome 2 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position: 

**Syntax**: `awk '$3 ==2' sorted_snp_position.txt | cut -f 1,3,4 > chrom_2_sorted_snp_position.txt`

Join the the two files with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ? for the chromosome 2: 

**Syntax**: `join -1 1 -2 1 chrom_2_sorted_snp_position.txt transposed_?_maize_genotype.txt > chrom_2_?_incr_posit_maize_genotype.txt`
#

- Chromosome 3: 
First, group sort the chromosome 3 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position: 

**Syntax**: `awk '$3 ==3' sorted_snp_position.txt | cut -f 1,3,4 > chrom_3_sorted_snp_position.txt`

Join the the two files with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ? for the chromosome 3: 

**Syntax**: `join -1 1 -2 1 chrom_3_sorted_snp_position.txt transposed_?_maize_genotype.txt > chrom_3_?_incr_posit_maize_genotype.txt`
#

- Chromosome 4: 
First, group sort the chromosome 4 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position: 

**Syntax**: ` awk '$3 ==4' sorted_snp_position.txt | cut -f 1,3,4 > chrom_4_sorted_snp_position.txt`

Join the the two files with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ? for the chromosome 4: 

**Syntax**: ` join -1 1 -2 1 chrom_4_sorted_snp_position.txt transposed_?_maize_genotype.txt > chrom_4_?_incr_posit_maize_genotype.txt`
#
- Chromosome 5: 
First, group sort the chromosome 5 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position: 

**Syntax**: `$ awk '$3 ==5' sorted_snp_position.txt | cut -f 1,3,4 > chrom_5_sorted_snp_position.txt`

Join the the two files with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ? for the chromosome 5:

**Syntax**: `join -1 1 -2 1 chrom_5_sorted_snp_position.txt transposed_?_maize_genotype.txt > chrom_5_?_incr_posit_maize_genotype.txt`
#
- Chromosome 6: 
First, group sort the chromosome 6 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position: 

**Syntax**: `awk '$3 ==6' sorted_snp_position.txt | cut -f 1,3,4 > chrom_6_sorted_snp_position.txt`

Join the the two files with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ? for the chromosome 6: 

**Syntax**:`join -1 1 -2 1 chrom_6_sorted_snp_position.txt transposed_?_maize_genotype.txt > chrom_6_?_incr_posit_maize_genotype.txt`
#

- Chromosome 7: 
First, group sort the chromosome 7 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position: 

**Syntax**: `awk '$3 ==7' sorted_snp_position.txt | cut -f 1,3,4 > chrom_7_sorted_snp_position.txt`

Join the the two files with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ? for the chromosome 7: 

**Syntax**: `join -1 1 -2 1 chrom_7_sorted_snp_position.txt transposed_?_maize_genotype.txt > chrom_7_?_incr_posit_maize_genotype.txt`
#
- Chromosome 8: 
First, group sort the chromosome 8 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position: 

**Syntax**: `awk '$3 ==8' sorted_snp_position.txt | cut -f 1,3,4 > chrom_8_sorted_snp_position.txt`

Join the the two files with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ? for the chromosome 8:

**Syntax**: ` join -1 1 -2 1 chrom_8_sorted_snp_position.txt transposed_?_maize_genotype.txt > chrom_8_?_incr_posit_maize_genotype.txt` 
#
- Chromosome 9: 
First, group sort the chromosome 9 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position: 

**Syntax**:`awk '$3 ==9' sorted_snp_position.txt | cut -f 1,3,4 > chrom_9_sorted_snp_position.txt`

Join the the two files with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ? for the chromosome 9:

**Syntax**: `join -1 1 -2 1 chrom_9_sorted_snp_position.txt transposed_?_maize_genotype.txt > chrom_9_?_incr_posit_maize_genotype.txt`
#
- Chromosome 10: 
First, group sort the chromosome 10 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position:

**Syntax**: ` awk '$3 ==10' sorted_snp_position.txt | cut -f 1,3,4 > chrom_10_sorted_snp_position.txt`

Join the the two files with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ? for the chromosome 10:

**Syntax**: `join -1 1 -2 1 chrom_10_sorted_snp_position.txt transposed_?_maize_genotype.txt > chrom_10_?_incr_posit_maize_genotype.txt`

#
**-10 files (1 for each chromosome) with SNPs ordered based on decreasing position values and with missing
data encoded by this symbol: -**
#

Sorting SNPs ordered based on decreasing position values: 

**Syntax**: `sort -k1,1V -k4,4nr sorted_snp_position.txt | cut -f 1,3,4 > decres_snp_position.txt`

Sorting missing data encoded by symbol:-

**Syntax**: `sed 's/?/-/g' transposed_?_maize_genotype.txt > transposed_-_maize_genotype.txt`
#


- Chromosome 1: 
First, group sort the chromosome 1 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position

**Syntax**: `awk '$3 ==1' decres_snp_position.txt | cut -f 1,3,4 > chrom_1_decres_snp_position.txt`


Join the the two files with SNPs ordered based on decreasing position values and with missing data encoded by this symbol: - for the chromosome 1: 

**Syntax**: `join -1 1 -2 1 chrom_1_decres_snp_position.txt transposed_-_maize_genotype.txt > chrom_1_-_decr_position_genotype.txt`
#

- Chromosome 2: 
First, group sort the chromosome 2 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position

**Syntax**: `aawk '$3 ==2' decres_snp_position.txt | cut -f 1,3,4 > chrom_2_decres_snp_position.txt`


Join the the two files with SNPs ordered based on decreasing position values and with missing data encoded by this symbol: - for the chromosome 1: 

**Syntax**: `join -1 1 -2 1 chrom_2_decres_snp_position.txt transposed_-_maize_genotype.txt > chrom_2_-_decr_position_genotype.txt`
#


- Chromosome 3: 
First, group sort the chromosome 3 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position

**Syntax**: `awk '$3 ==3' decres_snp_position.txt | cut -f 1,3,4 > chrom_3_decres_snp_position.txt`


Join the the two files with SNPs ordered based on decreasing position values and with missing data encoded by this symbol: - for the chromosome 1: 

**Syntax**: `join -1 1 -2 1 chrom_3_decres_snp_position.txt transposed_-_maize_genotype.txt > chrom_3_-_decr_position_genotype.txt`
#
- Chromosome 4: 
First, group sort the chromosome 4 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position

**Syntax**: `awk '$3 ==4' decres_snp_position.txt | cut -f 1,3,4 > chrom_4_decres_snp_position.txt`


Join the the two files with SNPs ordered based on decreasing position values and with missing data encoded by this symbol: - for the chromosome 1: 

**Syntax**: `join -1 1 -2 1 chrom_4_decres_snp_position.txt transposed_-_maize_genotype.txt > chrom_4_-_decr_position_genotype.txt`
#
- Chromosome 5: 
First, group sort the chromosome 5 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position

**Syntax**: `awk '$3 ==5' decres_snp_position.txt | cut -f 1,3,4 > chrom_5_decres_snp_position.txt`


Join the the two files with SNPs ordered based on decreasing position values and with missing data encoded by this symbol: - for the chromosome 1: 

**Syntax**: `join -1 1 -2 1 chrom_5_decres_snp_position.txt transposed_-_maize_genotype.txt > chrom_5_-_decr_position_genotype.txt`
#

- Chromosome 6: 
First, group sort the chromosome 6 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position

**Syntax**: `awk '$3 ==6' decres_snp_position.txt | cut -f 1,3,4 > chrom_6_decres_snp_position.txt`


Join the the two files with SNPs ordered based on decreasing position values and with missing data encoded by this symbol: - for the chromosome 1: 

**Syntax**: `join -1 1 -2 1 chrom_6_decres_snp_position.txt transposed_-_maize_genotype.txt > chrom_6_-_decr_position_genotype.txt`
#
- Chromosome 7: 
First, group sort the chromosome 7 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position

**Syntax**: ` awk '$3 ==7' decres_snp_position.txt | cut -f 1,3,4 > chrom_7_decres_snp_position.txt`


Join the the two files with SNPs ordered based on decreasing position values and with missing data encoded by this symbol: - for the chromosome 1: 

**Syntax**: `join -1 1 -2 1 chrom_7_decres_snp_position.txt transposed_-_maize_genotype.txt > chrom_7_-_decr_position_genotype.txt`
#
- Chromosome 8: 
First, group sort the chromosome 8 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position

**Syntax**: ` awk '$3 ==8' decres_snp_position.txt | cut -f 1,3,4 > chrom_8_decres_snp_position.txt`


Join the the two files with SNPs ordered based on decreasing position values and with missing data encoded by this symbol: - for the chromosome 1: 

**Syntax**: `join -1 1 -2 1 chrom_8_decres_snp_position.txt transposed_-_maize_genotype.txt > chrom_8_-_decr_position_genotype.txt`
#
- Chromosome 9: 
First, group sort the chromosome 9 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position

**Syntax**: ` awk '$3 ==9' decres_snp_position.txt | cut -f 1,3,4 > chrom_9_decres_snp_position.txt`


Join the the two files with SNPs ordered based on decreasing position values and with missing data encoded by this symbol: - for the chromosome 1: 

**Syntax**: `join -1 1 -2 1 chrom_9_decres_snp_position.txt transposed_-_maize_genotype.txt > chrom_9_-_decr_position_genotype.txt`
#
- Chromosome 10: 
First, group sort the chromosome 10 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position

**Syntax**: ` awk '$3 ==10' decres_snp_position.txt | cut -f 1,3,4 > chrom_10_decres_snp_position.txt`


Join the the two files with SNPs ordered based on decreasing position values and with missing data encoded by this symbol: - for the chromosome 1: 

**Syntax**: `join -1 1 -2 1 chrom_10_decres_snp_position.txt transposed_-_maize_genotype.txt > chrom_10_-_decr_position_genotype.txt`


#

 **-1 file with all SNPs with unknown positions in the genome**: 

Sorting SNPs with unkwon positions in the genome: 

**-Syntax**: `grep 'unknown' sorted_snp_position.txt | cut -f 1,3,4 > unkown_snp_positon.txt`

Join two files, unkown position and teosinte genotype files: 

**-Syntax**: `join -1 1 -2 1 unkown_snp_positon.txt transposed_maize_genotypes.txt > maize_unknown_genotype.txt`

#

#- Teosinte genotype: 
-10 files (1 for each chromosome) with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ?
#
- Chromosome 1: 
The chromosome 1 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position already sorted: 

**Syntax**: `awk '$3 ==1' sorted_snp_position.txt | cut -f 1,3,4 > chrom_1_sorted_snp_position.txt`


SNPs with missing data encoded by the symbol: ? 

**Syntax**: `grep '?' sorted_transposed_teosinte_genotypes.txt > transposed_?_teosinte_genotypes.txt`

Join the the two files with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ? for the chromosome 1: 

**Syntax**: ` join -1 1 -2 1 chrom_1_sorted_snp_position.txt transposed_?_teosinte_genotypes.txt > chrom_1_?_incr_posit_teosinte_genotype.txt`
#
- Chromosome 2: 
Chromosome 2 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position already sorted:  

**Syntax**: `awk '$3 ==2' sorted_snp_position.txt | cut -f 1,3,4 > chrom_2_sorted_snp_position.txt`

Join the the two files with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ? for the chromosome 2: 

**Syntax**: `join -1 1 -2 1 chrom_2_sorted_snp_position.txt transposed_?_teosinte_genotypes.txt  > chrom_2_?_incr_posit_teosinte_genotype.txt`
#
- Chromosome 3: 
The chromosome 3 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position already sorted: 

**Syntax**: `awk '$3 ==3' sorted_snp_position.txt | cut -f 1,3,4 > chrom_3_sorted_snp_position.txt`

Join the the two files with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ? for the chromosome 3: 

**Syntax**: `join -1 1 -2 1 chrom_3_sorted_snp_position.txt transposed_?_teosinte_genotypes.txt > chrom_3_?_incr_posit_teosinte_genotype.txt`
#
- Chromosome 4: 
The chromosome 4 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position: 

**Syntax**: ` awk '$3 ==4' sorted_snp_position.txt | cut -f 1,3,4 > chrom_4_sorted_snp_position.txt`

Join the the two files with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ? for the chromosome 4: 

**Syntax**: ` join -1 1 -2 1 chrom_4_sorted_snp_position.txt transposed_?_teosinte_genotypes.txt > chrom_4_?_incr_posit_teosinte_genotype.txt`
#
- Chromosome 5: 
The chromosome 5 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position sorted: 

**Syntax**: `$ awk '$3 ==5' sorted_snp_position.txt | cut -f 1,3,4 > chrom_5_sorted_snp_position.txt`

Join the the two files with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ? for the chromosome 5:

**Syntax**: `join -1 1 -2 1 chrom_5_sorted_snp_position.txt transposed_?_teosinte_genotypes.txt > chrom_5_?_incr_posit_teosinte_genotype.txt`
#
- Chromosome 6: 
The chromosome 6 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position sorted: 

**Syntax**: `awk '$3 ==6' sorted_snp_position.txt | cut -f 1,3,4 > chrom_6_sorted_snp_position.txt`

Join the the two files with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ? for the chromosome 6: 

**Syntax**:`join -1 1 -2 1 chrom_6_sorted_snp_position.txt transposed_?_teosinte_genotypes.txt > chrom_6_?_incr_posit_teosinte_genotype.txt`
#

- Chromosome 7: 
The chromosome 7 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position sorted: 

**Syntax**: `awk '$3 ==7' sorted_snp_position.txt | cut -f 1,3,4 > chrom_7_sorted_snp_position.txt`

Join the the two files with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ? for the chromosome 7: 

**Syntax**: `join -1 1 -2 1 chrom_7_sorted_snp_position.txt transposed_?_teosinte_genotypes.txt > chrom_7_?_incr_posit_teosinte_genotype.txt`
#
- Chromosome 8: 
The chromosome 8 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position sorted:

**Syntax**: `awk '$3 ==8' sorted_snp_position.txt | cut -f 1,3,4 > chrom_8_sorted_snp_position.txt`

Join the the two files with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ? for the chromosome 8:

**Syntax**: ` join -1 1 -2 1 chrom_8_sorted_snp_position.txt transposed_?_teosinte_genotypes.txt > chrom_8_?_incr_posit_teosinte_genotype.txt` 
#
- Chromosome 9: 
The chromosome 9 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position sorted: 

**Syntax**:`awk '$3 ==9' sorted_snp_position.txt | cut -f 1,3,4 > chrom_9_sorted_snp_position.txt`

Join the the two files with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ? for the chromosome 9:

**Syntax**: ` join -1 1 -2 1 chrom_9_sorted_snp_position.txt transposed_?_teosinte_genotypes.txt > chrom_9_?_incr_posit_teosinte_genotype.txt`
#
- Chromosome 10: 
First, group sort the chromosome 10 formatted as the first column is SNP_ID, the second colum is chromosome, the third column is position:

**Syntax**: ` awk '$3 ==10' sorted_snp_position.txt | cut -f 1,3,4 > chrom_10_sorted_snp_position.txt`

Join the the two files with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ? for the chromosome 10:

**Syntax**: `join -1 1 -2 1 chrom_10_sorted_snp_position.txt transposed_?_teosinte_genotypes.txt > chrom_10_?_incr_posit_teosinte_genotype.txt`
#

**-10 files (1 for each chromosome) with SNPs ordered based on decreasing position values and with missing
data encoded by this symbol: -**

Sorting SNPs ordered based on decreasing position values: 

**Syntax**: `sort -k1,1V -k4,4nr sorted_snp_position.txt | cut -f 1,3,4 > decres_snp_position.txt`

Sorting missing data encoded by symbol:-

**Syntax**: ` sed 's/?/-/g' transposed_?_teosinte_genotypes.txt > transposed_-_teosinte_genotype.txt`
#

- Chromosome 1 

**Syntax**: `join -1 1 -2 1 chrom_1_decres_snp_position.txt transposed_-_teosinte_genotype.txt > chrom_1_-_decr_position_genotype.txt`
#

- Chromosome 2 

**Syntax**: `join -1 1 -2 1 chrom_2_decres_snp_position.txt transposed_-_teosinte_genotype.txt > chrom_2_-_decr_position_genotype.txt`
#
- Chromosome 3 
**Syntax**: `join -1 1 -2 1 chrom_3_decres_snp_position.txt transposed_-_teosinte_genotype.txt > chrom_3_-_decr_position_genotype.txt`
#
- Chromosome 4
**Syntax**: `join -1 1 -2 1 chrom_4_decres_snp_position.txt transposed_-_teosinte_genotype.txt > chrom_4_-_decr_position_genotype.txt`
#
- Chromosome 5: 
**Syntax**: `join -1 1 -2 1 chrom_5_decres_snp_position.txt transposed_-_teosinte_genotype.txt > chrom_5_-_decr_position_genotype.txt`
#
- Chromosome 6: 
**Syntax**: `join -1 1 -2 1 chrom_6_decres_snp_position.txt transposed_-_teosinte_genotype.txt > chrom_6_-_decr_position_genotype.txt`
#
- Chromosome 7: 
**Syntax**: `join -1 1 -2 1 chrom_7_decres_snp_position.txt transposed_-_teosinte_genotype.txt > chrom_7_-_decr_position_genotype.txt`
#
_ Chromosome 8: 

**Syntax**: `join -1 1 -2 1 chrom_8_decres_snp_position.txt transposed_-_teosinte_genotype.txt > chrom_8_-_decr_position_genotype.txt`
#
- Chromosome 9: 
**Syntax**:`join -1 1 -2 1 chrom_9_decres_snp_position.txt transposed_-_teosinte_genotype.txt > chrom_9_-_decr_position_genotype.txt`
#
- Chromosome 10: 
**Syntax**: `join -1 1 -2 1 chrom_10_decres_snp_position.txt transposed_-_teosinte_genotype.txt > chrom_10_-_decr_position_genotype.txt`



#
**- File with all SNPs with unknown positions in the genome**: 

Sorting SNPs with unkwon positions in the genome: 

**-Syntax**: `grep 'unknown' sorted_snp_position.txt | cut -f 1,3,4 > unkown_snp_positon.txt`

Join two files, unkown position and teosinte genotype files: 

**-Syntax**: ``join -1 1 -2 1 unkown_snp_positon.txt transposed_teosinte_genotypes.txt > Teosinte_unkown_genotype`

