# Family-Filtering-Script

1. Download and install bgzip, tabix and VCF-tools:
http://www.htslib.org/
https://vcftools.github.io/index.html

2. Run bgzip on the files you plan on filtering:

  bgzip file1.vcf
  
  bgzip file2.vcf
  
  ... repeat for whole family
  
3. Run tabix on the now .gz files:

  tabix -p vcf File1.vcf.gz
  
  tabix -p vcf File2.vcf.gz
  
  ... repeat for whole family
 
4. run vcf-merge

  vcf-merge file1.vcf.gz file2.vcf.gz ... > merged.vcf
  
5. Utilize the family filtering script. Example execution code for a bash script:

MERGEDNAME=File1_File2
CHILD=L3p4d204
MOTHER=L3p5d204

cat  $MERGEDNAME.vcf | perl 
/Shared/CLCG/tbraun/temp/vcftools_0.1.9/perl/vcf-family-filter-4 --matches \
  --filter "WTaffected=$CHILD,-1,0" \
  --filter "HomHyp=$CHILD,2;$MOTHER,-1,1;*1,1" \
  --filter "Supported_M=$CHILD,1;$MOTHER,1" \
  --filter "MendelViolationR=$CHILD,2;*1,0" \
  --filter "MendelViolationD=$CHILD,1;*2,0" \
>  $MERGEDNAME.FamFilt.vcf

6. Remove unneccessary variants:

egrep –w ‘#|CHROM|HomHyp|Supported_M’ File1_File2.FamFilt.vcf > FF.Family.vcf
