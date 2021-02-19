---
layout: post
author: "subin"
title:  "Creating a Reference Package with CellRanger mkref"
subtitle: "How to make custom gene content reference data."
type: "10X"
category: "singleCell"
tags: ['10X','chromium','cellranger','mkref','reference','scRNA-seq에서 TdTomato 발현 여부 확인하기']
disqus: true
post-header: true
header-img: https://user-images.githubusercontent.com/43258282/108465761-f46f7100-72c5-11eb-8476-c72eab1db676.png
---

CellRanger에서는 대중적으로 많이 쓰이는 (GRCh38, mm10같은..)reference는 다운받아서 바로 사용할 수 있게 미리 만들어 제공하고 있다.[^1] 

다른 종이나 제공하지 않는 버전의 reference를 사용해서 분석하고 싶다면 <span style="background-color:#ffd966ff">**custom reference**</span>를 만들어서 cellranger pipeline에 먹이면 되는데, **reference genome sequence(FASTA file)와 gene annotation(GTF file) 정보**가 필요하다.[^2]

<span style="color:#ADADC9">*나는 TdTomato transgene positive한 cell만을 따로 보고 싶어서 제작함. 보통 이런경우에는 FACS로 sorting하고 sequencing하는 것 같긴 하지만 이미 만들었으니 어쨌든.. 만들어보자!* </span>

### Add a Marker Gene to the FASTA and GTF
나같은 경우에는 원래의 reference + α 가 필요한 케이스.[^3]
<br/> 10X tutorial에는 GFP를 예시로 나와있는데, TdTomato gene을 추가해보자.

사용한 cellrnager version: `cellranger-5.0.1`

위에서 언급했듯이 custom reference 만들기 위해서는 fasta file과 GTF file이 필요하다. 나는 cellranger에서 기본제공하는 mm10 reference에 TdTomato 정보를 추가해 완성할 생각이다.

**1. Download TdTomato sequence ** 
<br/> fasta file에 추가하기 위해 TdTomato sequence를 다운 받아[^4] *tdTomato.fa* 로 저장했다.
<details>
<div markdown="1">
<summary>TdTomato sequence: **tdTomato.fa** file</summary>

```
> tdTomato
ATGGTGAGCAAGGGCGAGGAGGTCATCAAAGAGTTCATGCGCTTCAAGGTGCGCATGGAG GGCTCCATGAACGGCCACGAGTTCGAGATCGAGGGCGAGGGCGAGGGCCGCCCCTACGAG GGCACCCAGACCGCCAAGCTGAAGGTGACCAAGGGCGGCCCCCTGCCCTTCGCCTGGGAC ATCCTGTCCCCCCAGTTCATGTACGGCTCCAAGGCGTACGTGAAGCACCCCGCCGACATC CCCGATTACAAGAAGCTGTCCTTCCCCGAGGGCTTCAAGTGGGAGCGCGTGATGAACTTC GAGGACGGCGGTCTGGTGACCGTGACCCAGGACTCCTCCCTGCAGGACGGCACGCTGATC TACAAGGTGAAGATGCGCGGCACCAACTTCCCCCCCGACGGCCCCGTAATGCAGAAGAAG ACCATGGGCTGGGAGGCCTCCACCGAGCGCCTGTACCCCCGCGACGGCGTGCTGAAGGGC GAGATCCACCAGGCCCTGAAGCTGAAGGACGGCGGCCACTACCTGGTGGAGTTCAAGACC ATCTACATGGCCAAGAAGCCCGTGCAACTGCCCGGCTACTACTACGTGGACACCAAGCTG GACATCACCTCCCACAACGAGGACTACACCATCGTGGAACAGTACGAGCGCTCCGAGGGC CGCCACCACCTGTTCCTGGGGCATGGCACCGGCAGCACCGGCAGCGGCAGCTCCGGCACC GCCTCCTCCGAGGACAACAACATGGCCGTCATCAAAGAGTTCATGCGCTTCAAGGTGCGC ATGGAGGGCTCCATGAACGGCCACGAGTTCGAGATCGAGGGCGAGGGCGAGGGCCGCCCC TACGAGGGCACCCAGACCGCCAAGCTGAAGGTGACCAAGGGCGGCCCCCTGCCCTTCGCC TGGGACATCCTGTCCCCCCAGTTCATGTACGGCTCCAAGGCGTACGTGAAGCACCCCGCC GACATCCCCGATTACAAGAAGCTGTCCTTCCCCGAGGGCTTCAAGTGGGAGCGCGTGATG AACTTCGAGGACGGCGGTCTGGTGACCGTGACCCAGGACTCCTCCCTGCAGGACGGCACG CTGATCTACAAGGTGAAGATGCGCGGCACCAACTTCCCCCCCGACGGCCCCGTAATGCAG AAGAAGACCATGGGCTGGGAGGCCTCCACCGAGCGCCTGTACCCCCGCGACGGCGTGCTG AAGGGCGAGATCCACCAGGCCCTGAAGCTGAAGGACGGCGGCCACTACCTGGTGGAGTTC AAGACCATCTACATGGCCAAGAAGCCCGTGCAACTGCCCGGCTACTACTACGTGGACACC AAGCTGGACATCACCTCCCACAACGAGGACTACACCATCGTGGAACAGTACGAGCGCTCC GAGGGCCGCCACCACCTGTTCCTGTACGGCATGGACGAGCTGTACAAGTAA
```

</div>
</details>
<br/>

**2. Make TdTomato .fa & .gtf** <br/>
tdTomato가 몇개의 base로 이루어졌는지 알아보자 = 1,431 bases
```
cat tdTomato.fa | grep -v "^>" | tr -d "\n" | wc -c
```
이제 이 정보를 활용해서 tdTomato.gtf file을 만들어보자. GTF file에는 총 9가지 column이 필요하다. <span style="color:#EC7063">\t</span>
로 구분하자.
```
echo -e 'tdTomato\tunknown\texon\t1\t1431\t.\t+\t.\tgene_id "tdTomato"; transcript_id "tdTomato"; gene_name "tdTomato"; gene_biotype "protein_coding";' > tdTomato.gtf
```
<details>
<div markdown="1">
<summary>**tdTomato.gtf** file</summary>
만들면 이렇게 된다.
```
tdTomato	unknown	exon	1	1431	.	+	.	gene_id "tdTomato"; transcript_id "tdTomato"; gene_name "tdTomato"; gene_biotype "protein_coding";
```
</div>
</details>
<br/>

**3. Add TdTomato .fa & .gtf to the end of the mm10 reference file** <br/>
추가 하기전에 원래 있던 데이터가 손상되지 않게 복사본을 만들어주자.
```
cp cellranger-5.0.1/refdata-gex-mm10-2020-A/fasta/genome.fa mm10-2020-A-w-tdTomato_genome.fa
cp cellranger-5.0.1/refdata-gex-mm10-2020-A/genes/genes.gtf mm10-2020-A-w-tdTomato_genes.gtf
```
아까 만들어둔 tdTomato fasta file과 gtf file을 아래 붙여준다.
```
cat tdTomato.fa >> mm10-2020-A-w-tdTomato_genome.fa
grep ">" mm10-2020-A-w-tdTomato_genome.fa  // 잘 붙었는지 확인해보자

cat tdTomato.gtf >> mm10-2020-A-w-tdTomato_genes.gtf
tail mm10-2020-A-w-tdTomato_genes.gtf  // 잘 붙었는지 확인해보자
```
<br/>
**4. Make custom reference** <br/>
지금까지 만든 .fa와 .gtf file을 사용해서 Cellranger `count` 단계에서 쓸 custom reference를 완성해보자! `cellranger mkref`를 사용한다.
```
cellranger mkref --genome=mm10-2020-A-w-tdTomato \
--fasta=mm10-2020-A-w-tdTomato_genome.fa \
--genes=mm10-2020-A-w-tdTomato_genes.gtf
```

완성되면 `cellranger count` 에서 `--transcriptome=` 옵션에 reference 만들어둔 dir 넣어주면 끝!
<br/><br/><br/>

[^1]: [CellRanger reference 다운로드 바로가기](https://support.10xgenomics.com/single-cell-gene-expression/software/downloads/latest)<br/>이름과 소속등을 입력하고 term에 동의해야 가능할 것이다.
[^2]: [10X genomics: Creating a Reference Package with cellranger mkref](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/advanced/references#header)
[^3]: [10X genomics: Build a Custom Reference With cellranger mkref](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/using/tutorial_mr)
[^4]: [SnapGene: TdTomato](https://www.snapgene.com/resources/plasmid-files/?set=fluorescent_protein_genes_and_plasmids&plasmid=tdTomato)



