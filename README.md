# hse21_hw1
## Домашняя работа 1 по майнору Биоинформатика
Ход работы
1. Создание ссылок в папке hw1
    ```
    /usr/share/data-minor-bioinf/assembly | xargs -I % -t ln -s /usr/share/data-minor-bioinf/assembly/%
    ```
2. Выбираем случайные сэмплы
    ```
    seqtk sample -s427 oil_R1.fastq 5000000 >paired_end_1.fastq
    seqtk sample -s427 oil_R2.fastq 5000000 >paired_end_2.fastq
    seqtk sample -s427 oilMP_S4_L001_R1_001.fastq 1500000 > mate_pairs_1.fastq
    seqtk sample -s427 oilMP_S4_L001_R2_001.fastq 1500000 > mate_pairs_2.fastq
    ```
3. Считаем статистику с помощью fastqc
    ```
    ls *_?.fastq | xargs -t -I % fastqc -o fastqc %
    ```
4. Объединяем с multiqc
    ```
    multiqc -o muliqc fastqc
    ```
