# Qualidade do Sequenciamento

## Sequence Read Archive (SRA)
Banco de dados para dados de sequenciamento de DNA (*short reads*/sequências com menos de 1000 pares).

## Formato FASTQ
Formato de arquivo para leituras de sequeciamento (quebrar o genoma e montar) geradas a partir de NGS (Next Generation Sequencing). Evoluiu do FASTA e contém dados da sequência e informações de qualidade.
 
**fastsanger**: escala de codificação de qualidade.
```
Quality encoding: ! " # $ % & ' ( ) * + , - . / 0 1 2 3 4 5 6 7 8 9 : ; < = > ? @ A B C D E F G H I
                  |                   |                   |                   |                   |
Quality score:    0 1 2 3 4 5 6 7 8 9 10. . . . . . . . . 20. . . . . . . . . 30. . . . . . . . . 40
```

Cada pontuação de qualidade representa a probabilidade de um nucleotídeo correspondente estar incorreto, onde $Q = -10 \times \log(P)$. Quanto maior o quality score, maior o base call accuracy (menor a change de estar incorreto). Geralmente, um alinhamento é considerado bom com $Q > 20$.

Dado um código de uma sequência, podemos rodar o seguinte código para visualizar o arquivo fastq:
```bash
prefetch SRR2079547
cd SRR2079547/
fastq-dump SRR2079547.rsa
head SRR2079547.fastq
```

## FASTQC
Programa para verificar a qualidade das reads no arquivo fastq. 

## EA-Utils
Programa para processamento de reads. Verifica um arquivo de sequência em busca de adaptadores e determina um conjunto de parâmetros de corte. Também faz detecção de distorção e filtragem de qualidade.
```bash
cd clipper/
./fastq-mcf -q 20 n/a ..[PATH]/SRR2079547/SRR2079547.fastq -o ..[PATH]/SRR2079547/SRR2079547_Q20.fastq
```
Podemos comparar o arquivo original e cortado no FASTQC.
```bash
cd clipper/
./fastq-mcf -q 20 -L 120 n/a ..[PATH]/SRR2079547/SRR2079547.fastq -o ..[PATH]/SRR2079547/SRR2079547_Q20_L120.fastq
```