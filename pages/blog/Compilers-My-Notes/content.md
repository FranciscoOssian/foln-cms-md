# Esclarecimento

Este conteúdo publico não é uma recomendação de estudo, mas sim uma vontade do autor de deixar publico seus estudos e gostos pela área.

## fontes

- [Compiladores - Judson Santiago - Youtube](https://www.youtube.com/playlist?list=PLX6Nyaq0ebfhI396WlWN6WlBm-tp7vDtV)
- [Anatomia de um compilador - cs.man.ac.uk](https://www.cs.man.ac.uk/~pjj/farrell/comp3.html)
- [Análise léxica - wikipédia](https://pt.wikipedia.org/wiki/An%C3%A1lise_l%C3%A9xica)

# Compiladores

Os compiladores utilizam muitos algoritmos e estruturas de dados como:

- Buscas heurísticas gulosas (alocação de registradores)
- Autômatos finitos determinísticos (reconhecer palavras na entrada)
- Algoritmos de ponto fixo (análise de fluxo de dados)
- Provadores de teorema e simplificadores algébricos (para prever valores de expressões)
- Reconhecimento de padrão (mapear computações abstratas em operações em nível de máquina)
- Equações diofantinas lineares e aritmética de Pressburger (analisar subscritos de vetores)
- Algoritmos em grafos (eliminação de código morto)
- Pilhas (analise sintática shift-reduce)
- Tabelas de dispersão (hash table)
- Algoritmos clássicos (buffers e estruturas de dados diversas)

Se eu já sei tudo isso? É CLARO QUE NÃO :-)
Espero poder saber até terminar de escrever isso (provável que não)

## Anatomia de um compilador

> Um compilador é uma ferramenta que traduz um programa de uma linguagem para outra

A anatomia com o mínimo de detalhe possível é uma que tem apenas entrada e saída. O front do compilador e o Back do compilador. Pelo front é posto o código texto fonte, e pelo back é saído o código objeto ou alvo.

Sobre a estrutura do compilador a primeira etapa é a de Análise léxica / Lexer

> Na ciência da computação, análise léxica, lexing ou tokenização é o processo de converter uma sequência de caracteres (como em um programa de computador ou página da web) em uma sequência de tokens (strings com um significado atribuído e, portanto, identificado). Um programa que realiza análise lexical pode ser denominado lexer, tokenizer, ou scanner, embora scanner também seja um termo para o primeiro estágio de um lexer. Um lexer geralmente é combinado com um analisador, que juntos analisam a sintaxe de linguagens de programação, páginas da Web e assim por diante.

### Token

> Um Token em computação é um segmento de texto ou símbolo que pode ser manipulado por um analisador sintático, que fornece um significado ao texto; em outras palavras, é um conjunto de caracteres (de um alfabeto, por exemplo) com um significado coletivo.Tokens são os padrões que ocorrem em uma string, por exemplo: em uma data, 29/03/1991, poderia utilizar dois tokens para dividir a string em três partes, utilizando a barra / como padrão. Desse modo, qualquer data que for inserida poderá ser dividida e analisada separadamente em dia, mês e ano. O mesmo pode ser feito com expressões regulares, mas o método de tokens utiliza muito menos processamento, e, portanto, é mais rápido, apesar de não ser tão robusto.

### O Lexer

O Loxer vai gerar os tokens e passar para o analisador sintático

### O analisador sintático

Ele vai receber os tokens e gerar uma AST, uma árvore de de sintaxe abstrata e vai passar para o analisar semântico.

### O analisador semântico

Ele vai receber a AST e gerar uma nova AST, corrigida pois a gerada pela análise sintática pode apresentar erros, ela geralmente não trata tipos e erros semânticos em geral.

### O gerador de código intermediário

Ele vai pegar essa AST e gerar uma interpretação intermediária

### Otimização

Uma etapa de otimização é usada, ela recebe a representação intermediária e gera uma nova IR, porém otimizada.

### Geração de código

Essa etapa receba a IR otimizada e gera o código.

### otimiador de código de máquina

Como o nome já diz, recebe o e otimiza. No caso vai receber o código e otimizar.

# dps escrevo mais