# hse21_hw1
## Домашняя работа 1 по майнору Биоинформатика
### Сборка генома
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
    multiqc -o multiqc fastqc
    ```
5. Подрезаем адаптеры
    ```
    platanus_trim paired_end_1.fastq paired_end_2.fastq
    platanus_internal_trim mate_pairs_1.fastq mate_pairs_2.fastq
    ```
6. Собираем анализ
    ```
    ls *.trimmed | xargs -t -I % fastqc -o fastqc %
    multiqc -o muliqc fastqc
    ```
7. Собираем контиги
    ```
    platanus assemble -o Poil -t 1 -m 10 -f *.trimmed 2>assemble.log
    ```
8. Объединяем контиги
    ```
    platanus scaffold -o Poil -t 1 -c Poil_contig.fa -IP1 *.trimmed -OP2 *.int_trimmed 2>scaffold.log
    ```
9. Находим наидленнейшую последовательность
    ```
    echo scaffold1_len3834874_cov232 > tmp.txt
    seqtk subseq Poil_scaffold.fa tmp.txt >longest_with_gaps.fasta
    ```
10. Закрываем пропуски
    ```
    platanus gap_close -o Poil -t 1 -c Poil_scaffold.fa -IP1 *.trimmed -OP2 *.int_trimmed 2>gapclose.log
    ```
11. Опять находим наидлиннейшую подпоследовательность

### Картинки из multiqc
Без обрезания адаптеров
![multiqc](/images/report_1.png)
![multiqc](/images/report_2.png)
![multiqc](/images/report_3.png)
С обрезанием адаптеров
![multiqc for trimmed](/images/report_trimmed_1.png)
![multiqc for trimmed](/images/report_trimmed_2.png)
![multiqc for trimmed](/images/report_trimmed_3.png)
Полный отчёт находится в файлах __multiqc_report.html__ и __multiqc_report_trimmed.html__.

### Анализ последовательностей
Анализ собранных последовательностей находится в ноутбуке в папке src.
