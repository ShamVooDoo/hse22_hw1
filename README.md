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
1) Оригинальный MultiQC
![image](https://user-images.githubusercontent.com/1662453/194418385-4f9e3960-1980-4fed-ae34-c1f85f5facce.png)
![image](https://user-images.githubusercontent.com/1662453/194418602-1dc6a3c7-bb5d-423f-b213-eb028d7db86a.png)
2) Подрезанный MultiQC
![image](https://user-images.githubusercontent.com/1662453/194418964-7b592ba0-4444-436b-bf3d-1fc5d3e3c035.png)
![image](https://user-images.githubusercontent.com/1662453/194419006-1dd3e28a-11a9-4f01-bace-fb808bc43e4e.png)


