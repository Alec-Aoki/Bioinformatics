# Alinhamento de Sequências
Arranjar sequências de DNA, RNA ou proteínas procurando regiões de similaridade e identidade.
- Identificar funções
  - *Genes análogos*: funções idênticas/semelhantes, mas não compartilham ancestral comum
- Identificar genes homólogos (ancestral comum)
  - *Sequências hortólogas*: seq. homólogas cujo último evento evolutivo foi a criação de uma nova espécie (seq. similares em espécies diferentes)
  - *Sequências parálogas*: " que divergiram dentro de uma mesma espécie (seq. similares na mesma espécie)
  - *Sequências xenólogas*: sequências semelhantes entre organismos distantemente relacionados (transferência horizontal de genes)
- Montar genomas

## Similaridade x Identidade
Para DNA e RNA, são sinônimos.

Para proteínas:
- Identidade: % de correspondências exatas
- Similaridade: % de resíduos alinhados que compartilham característica semelhantes

Exemplo:
- A: AAGGCTT
- B: AAGGC
- C: AAGGCAT
- Ident(A,B) = 100% ```(5 nucleotideos idênticos / min(comprimento(A), comprimento(B)))```
- Ident(B,C) = 100%
- Ident(A,C) = 85% ```(6 nucleotídeos / 7)```

Identidade = 100% não significa que as sequências são iguais!

## Componentes de Alinhamento
- *Match*: alinhamento, sequências iguais (pontua)
- *Mismatch*: alinhamento, mas sequências diferentes (penalizado)
- *Gaps*: buracos entre duas sequências que podem ser alinhadas (penalizado)

A posição do gap pode influenciar nos mismatches!

## Tipos de Alinhamento
- **Alinhamento Global**
  - Seq. inteira
  - Melhor pra seq. relacionadas
  - *Needleman-Wunsch*
- **Alinhamento Local**
  - Regiões de alta similariade (subseq.)
  - Melhor para seq. divergentes
  - *Smith-Waterman*

Caso haja muitas regiões de alinhamento ruim (muitos gaps/regiões de pouca similariade), comum entre espécies distantes, é melhor usar alinhamento local.

### Needleman-Wunsch
Matriz entre as duas sequências.
1. Inicializar a matriz MxN
2. Preencher a matriz do canto sup. esq. até o canto inf. dir. de maneira recursiva, usando o esquema de pontuação
   1. Guardar que cédulas anteriores a cédula atual usou (para o traceback)
3. Traceback
   1. Começa do canto inf. direito
   2. A sequência final é formada do canto sup. esq. até o canto inf. dir.
      1. Caso a seta aponte para a esquerda, há gap em $S_2$, caso aponte para cima, há gap em $S_1$

Para cada cédula ```(i,j)```, seu valor é ```max( (i-1,j) + gapPenalty, (i,j-1) + gapPenalty, (i-1,j-1) + (match || mismatch) )```, considerando o esquema de pontuação (gapPenalty < 0).

### Smith-Waterman
Similar a N.W., mas:
- A coluna e linha 0 são preenchidas com 0
- Há uma quarta pontuação (0)
  - Evita valores negativos, já que se tiver só valor negativo o max. vai ser o 0
- Começamos o traceback do maior valor
  - Depois podemos pegar o próximo maior valor (múltiplos alinhamentos locais)
  - OBS: caso o valor seja 0 não precisamos guardar a cédula que deu origem
- Percorremos enquanto a pontuação for > 0