# hse22_hw1
# Обязательная часть
*Создание ссылок*
```
ln -s /usr/share/data-minor-bioinf/assembly/oil_R1.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oil_R2.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R1_001.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R2_001.fastq
```
*Выбор случайных чтений:*
```
seqtk sample -s116 oil_R1.fastq 5000000 > sub1.fastq
seqtk sample -s116 oil_R2.fastq 5000000 > sub2.fastq
seqtk sample -s116 oilMP_S4_L001_R1_001.fastq 1500000 > mps1.fastq
seqtk sample -s116 oilMP_S4_L001_R2_001.fastq 1500000 > mps2.fastq
```
*Оценка качества и статистика по исходным чтениям*
Создаём папки с результатами работы fastQC и multiQC:  
```
mkdir hw1-fastqc-results && mkdir hw1-multiqc-results
```
Получаем чтения с помощью fastQc:
```
ls sub* mps* | xargs -tI{} fastqc -o \hw1-fastqc-results {}

```
Выполняем анализ через multiQC:
```
multiqc -o \hw1-multiqc-results \hw1-fastqc-results
```
Результаты для неподрезанных чтений:
```
![image1](https://github.com/whiteroomlz/hse22_hw1/blob/images/non-trimmed-general-report.png)
![image2](https://github.com/whiteroomlz/hse22_hw1/blob/images/non-trimmed-sequence-quality-scores.png)
```
Подрезаем чтения:
```
platanus_trim sub*
```
Удаляем адаптеры:
```
platanus_internal_trim mps*
```
Удаляем исходные файлы:
```
rm sub1.fastq sub2.fastq mps1.fastq mps2.fastq
```
Создаём папки статистик для подрезанных данных:
```
mkdir hw1-fastqc-trimmed-results && mkdir hw1-multiqc-trimmed-results
```
Снова запускаем fastQC...:
```
ls sub* mps*| xargs -tI{} fastqc -o \hw1-fastqc-trimmed-results {}
```
...и multiQC:
```
multiqc -o \hw1-multiqc-trimmed-results \hw1-fastqc-trimmed-results
```
Результаты для подрезанных чтений:
```
![image3](https://github.com/whiteroomlz/hse22_hw1/blob/images/trimmed-general-report.png)
![image4](https://github.com/whiteroomlz/hse22_hw1/blob/images/trimmed-sequence-quality-scores.png)
```
