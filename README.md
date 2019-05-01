# Identification and Characterization of Occult Human-specific LINE-1 Insertions using Long-read Sequencing Technology
## BLASR
```
#for PacBio raw read
blasr ./SRR_hdf5/SRR.fofn hs37d5.fa -sa hs37d5.blasr.sa -clipping soft -sam -out SRR.sam

#for alignment of error corrected reads
blasr ./correctedReads.fasta region.fa --sam --clipping soft --out region.sam 
```

## Canu
```
canu useGrid=false -correct -d ./region/canu.0510 -p region.date genomeSize=60k -pacbio-raw ./region/output.fastq
```

## MELT
```
#for NA12878 Illumina platimun data
java -jar ./MELTv2.0.2/MELT.jar Single -l NA12878.bam  -w /workdir -t ./MELTv2.0.2/me_refs/1KGP_Hg19/LINE1_MELT.zip -h hs37d5.fa -n ./MELTv2.0.2/add_bed_files/1KGP_Hg19/hg19.genes.bed -c 49 -b hs37d5

#for single-cell experiment (scWGS59)
java -jar ./MELTv2.0.2/MELT.jar Single -l 82759.bam -w /workdir -t ./MELTv2.0.2/me_refs/1KGP_Hg19/LINE1_MELT.zip -h hs37d5.fa -n ./MELTv2.0.2/add_bed_files/1KGP_Hg19/hg19.genes.bed -b hs37d5 -c 21
```

## jellyFish
```
#for establishing 26mers index
jellyfish count -m 26 -s 5G -t 10 -o jf.index.26kmers <(zcat ./ERR194147_1.fastq.gz) <(zcat ./ERR194147_2.fastq.gz) <(zcat ./ERR194147.fastq.gz)


#for kmer counting
jellyfish query jf.index.26kmers 26mer.list.in.reads >> count.26.NA12878.0824.txt
```

## PALMER
```
#Frozen vesion for the project at https://github.com/mills-lab/PALMER/releases/tag/V1.3.0
PALMER --input ./alignment_hs37d5/NA12878.washu.alignment_hs37d5.${i}.bam --workdir ./chr${i}.line.1127.11.a/ --ref_ver GRCh37 --output NA12878.chr${i} --chr chr${i} --ref_fa ./reference/hs37d5/hs37d5.fa --type LINE
```
