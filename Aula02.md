# Alinhamento de Várias Sequências
**BLAST**: Ferramenta de busca que usa algoritmos de alinhamento. Quebra as sequências em palavras (*k-mers*/substrings de tamanho *k*) e faz hash de suas localizações pra acelerar pesquisas seguintes; compara os k-mers e só retorna aqueles que uma pontuação superior a um limiar.

Para cada trecho de alta pontuação, tenta estender a sequência para ambos os lados até a pontuação cair abaixo do limiar (*Seed and Extend*), formando *High-scoring Segment Pairs* (HSP). Usa o algoritmo Smith-Waterman pra unir os HSP e obter um alinhamento final bom.

Ainda mais, permuta a query original encontrada e busca por alinhamentos novamente. Se muitos alinhamentos foram encontrados, o alinhamenot original tem pouca significância estatística. Se poucos alinhamentos foram encontrados, então há maior significância (quanto **menor** o *E-value*, maior a similaridade entre as proteínas). Esse valor é calculado normalizando o raw score baseado no tamanho da query e da base dados.

## Família de Algoritmos de Alinhamento do BLAST
- **DNA x DNA:** BLASTn, tBLASTx
- **DNA x Proteína**: BLASTx
- **Proteína x DNA**: tBLASTn
- **Proteína x Proteína**: BLASTp

# Alinhamento de DNA x Proteínas
Diferentes códons (3 bases) podem codificar o mesmo aminoácido, mas pequenas modificações em um códon geram aminoácidos completamente diferentes. Mudar um único nucleotídeo pode mudar completamente o formato de uma molécula.

**Matrizes de Pontuação**: formadas empiricamente. Mais usadas: *PAM* e *BLOSUM*.

## PAM (Point Accepted Mutation)
Margaret Dayhoff (1965).

Compilação de alinhamentos de sequências, verificando frequências de mudanças em aminoácidos. Baseado na divergência evolutiva entre sequências.

Cada linha e coluna da matriz representa um dos 20 aminoácidos padrão, e a matriz é como uma matriz de substituição para pontuar alinhamentos entre sequências de acordo com sua aceitação pela evolução/seleção natural. Quanto maior a pontuação, mais semelhante.

Também foi calculado a *mutabilidade relativa* (quantidade de vezes que cada aminoácido é substituído / quantidade de vezes que ocorre em um intervalo). A partir disso é possível gerar uma matriz de probabilidade de mutação de acordo com o intervalo evolutivo. A *PAM1* é a matriz para 1 intervalo evolutivo. *PAM2* = *PAM1* x *PAM1*, e assim por diante*, mantendo *PAM1* sempre à esquerda (**pesquisar**).

## BLOSUM (Blocks Substitution Matrix)
Henikoff e Henikoff (1992).

Analisaram regiões conservadas (sem gaps) entre proteínas do banco BLOCKS, contaram a quantidade de matches e mismatches and criaram a matriz de similaridade. Além disso, dividiram os grupos em subgrupos por similaridade. A BLOSUM62 é usada no BLAST.

## PAM x BLOSUM
Conforme aumentamos o número da PAM, temos mais mutaçõs a cada 100 aminoácidos. Conforme aumentamos a BLOSUM, temos maior similaridade.