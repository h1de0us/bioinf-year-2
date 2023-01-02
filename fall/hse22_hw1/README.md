Козлова Екатерина, группа 2

## Делаем символические ссылки на файлы
```
ln -s /usr/share/data-minor-bioinf/assembly/oil_R1.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oil_R2.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R1_001.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R2_001.fastq
```

## Сэмплы через seqtk
```
seqtk sample -s 1230 oil_R1.fastq 5000000 > sub1.fastq
seqtk sample -s 1230 oil_R2.fastq 5000000 > sub2.fastq
seqtk sample -s 1230 oilMP_S4_L001_R1_001.fastq 1500000 > mp1.fastq
seqtk sample -s 1230 oilMP_S4_L001_R2_001.fastq 1500000 > mp2.fastq
```

## Анализ через fastqc и multiqc
```
mkdir fastqc
mkdir multiqc

fastqc -o fastqc sub1.fastq 
fastqc -o fastqc sub2.fastq
fastqc -o fastqc mp1.fastq
fastqc -o fastqc mp2.fastq

multiqc -o multiqc fastqc
```
![untrimmed1](https://github.com/h1de0us/hse22_hw1/blob/main/untrimmed1.png)
![untrimmed2](https://github.com/h1de0us/hse22_hw1/blob/main/untrimmed2.png)

## Обрезаем через platanus
```
platanus_trim sub1.fastq sub2.fastq
platanus_internal_trim mp1.fastq mp2.fastq
```

## Избавляемся от лишних файлов
```
rm sub1.fastq
rm sub2.fastq
rm mp1.fastq
rm mp2.fastq
```


## Анализ обрезанных файлов через fastqc и multiqc
```
mkdir fastqc_trimmed
mkdir multiqc_trimmed

fastqc -o fastqc_trimmed sub1.fastq.trimmed
fastqc -o fastqc_trimmed sub2.fastq.trimmed
fastqc -o fastqc_trimmed mp1.fastq.int_trimmed
fastqc -o fastqc_trimmed mp2.fastq.int_trimmed

multiqc -o multiqc_trimmed fastqc_trimmed
```
![trimmed1](https://github.com/h1de0us/hse22_hw1/blob/main/trimmed1.png)
![trimmed2](https://github.com/h1de0us/hse22_hw1/blob/main/trimmed2.png)


## Копирую всё себе локально

## Анализ через platanus
```
time platanus assemble -o Poil -f sub1.fastq.trimmed sub2.fastq.trimmed 2> assemble.log
time platanus scaffold -o Poil -c Poil_contig.fa -IP1 sub1.fastq.trimmed sub2.fastq.trimmed -OP2 mp1.fastq.int_trimmed mp2.fastq.int_trimmed 2> scaffold.log
time platanus gap_close -o Poil -c Poil_scaffold.fa -IP1 sub1.fastq.trimmed sub2.fastq.trimmed -OP2 mp1.fastq.int_trimmed mp2.fastq.int_trimmed 2> gapclose.log
```

## Снова удаляем лишние файлы
```
rm sub1.fastq.trimmed
rm sub2.fastq.trimmed
rm mp1.fastq.int_trimmed
rm mp2.fastq.int_trimmed
```
// тут я ещё удаляла символические ссылки, чтобы сделать scp

## Снова копирую 
```
rm -rf hw1 // чищу папку, чтобы поработать c меньшим числом чтений
// размер для sub - 500000, размер для mp - 150000
// дальше просто прогоняю те же команды, что и для основного задания
```

## Результаты анализа контигов, скаффолдов и гэпов:
```
Информация о контигах:
Количество: 631
Длина: 3927078
Максимальная длина: 3904746
Подсчитанное N50: 3904746

Информация о скаффолдах:
Количество: 70
Длина: 3873604
Максимальная длина: 3873500
Подсчитанное N50: 3873500

Информация о гэпах в случае необрезанных чтений:
Длина всех гэпов: 6284
Количество гэпов: 72

Информация о контигах в случае обрезанных чтений:
Количество: 70
Длина: 3919726
Максимальная длина: 3919622
Подсчитанное N50: 3919622
Информация о гэпах в случае обрезанных чтений:
Длина всех гэпов: 1833
Количество гэпов: 9
```

## Результаты анализа контигов, скаффолдов и гэпов для бонусного задания:
```
Информация о контигах:
Количество: 3433
Длина: 3916770
Максимальная длина: 3916737
Подсчитанное N50: 3916737
Информация о гэпах максимального контига:
Длина всех гэпов: 0
Количество гэпов: 0


Информация о скаффолдах:
Количество: 464
Длина: 3865231
Максимальная длина: 3864990
Подсчитанное N50: 3864990

Информация о гэпах в случае необрезанных чтений:
Длина всех гэпов: 75238
Количество гэпов: 1653

Информация о контигах в случае обрезанных чтений:
Количество: 464
Длина: 3856053
Максимальная длина: 3855812
Подсчитанное N50: 3855812
Информация о гэпах в случае обрезанных чтений:
Длина всех гэпов: 30842
Количество гэпов: 113
```
