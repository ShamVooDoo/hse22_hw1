## hse22_hw1 Юферов Матвей
1) seed:1126 Выборка  
```
ln -s /usr/share/data-minor-bioinf/assembly/oil_R1.fastq 
seqtk sample -s1126 oil_R1.fastq 50000000 > R1PairedEnd.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oil_R2.fastq
seqtk sample -s1126 oil_R2.fastq 50000000 > R2PairedEnd.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R1_001.fastq
seqtk sample -s1126 oilMP_S4_L001_R1_001.fastq 1500000 > R1MatePairs.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R2_001.fastq 
seqtk sample -s1126 oilMP_S4_L001_R2_001.fastq 1500000 > R2MatePairs.fastq
```
2) fastQC и multiQC 
```
mkdir fastqc
ls R* | xargs -P 4 -tI{} fastqc -o fastqc {}
```
```
mkdir multiqc
multiqc -o multiqc fastqc
```
3) platanus обрезание выборки 
```
platanus_trim R*PairedEnd.fastq
platanus_internal_trim R*MatePairs.fastq
```
4) fastQC и multiQC для обрезанной выборки (старые файлы удалил)
```
mkdir fastqcTrim
ls R* | xargs -P 4 -tI{} fastqc -o fastqcTrim {}
```
```
mkdir multiqcTrim
multiqc -o multiqcTrim fastqcTrim
```
5) Контиги
```
platanus assemble -o Poil -f R1PairedEnd.fastq.trimmed R2PairedEnd.fastq.trimmed 2> assemble.log
```
6) Скаффолды
```
platanus scaffold -o Poil -c Poil_contig.fa -IP1 R1PairedEnd.fastq.trimmed R2PairedEnd.fastq.trimmed -OP2 R1MatePairs.fastq.int_trimmed R2MatePairs.fastq.int_trimmed 2> scaffold.log
```
7) Уменьшаем гэпы
```
platanus gap_close -o Poil -c Poil_scaffold.fa -IP1 R1PairedEnd.fastq.trimmed R2PairedEnd.fastq.trimmed -OP2 R1MatePairs.fastq.int_trimmed R2MatePairs.fastq.int_trimmed 2> gapclose.log
```
## Отчеты
