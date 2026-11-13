# Aula Prática 8 - Modelação Hierárquica (continuação)

## Introdução

Esta aula é uma segunda oportunidade para a
[Aula Prática 7](./lab07-pt.md). Não há exercícios novos: use-a para
terminar os exercícios ex23-ex28, para esclarecer o que tenha ficado por
perceber e para garantir que domina a modelação hierárquica antes de
precisar dela no Projeto 2, que depende muito dela.

Se ainda não começou, parta do
[código inicial](../../src/labs/ex23-step0/) e da secção Preparação da
Aula 7.

## Por onde continuar

Retome a partir do primeiro exercício que ainda não terminou:

- **ex23-ex24** (Sol, Mercúrio e Vénus): o mínimo para esta aula é um sistema
  solar com estes três astros a funcionar, desenhado a partir de um grafo de
  cena que desenhou primeiro no papel.
- **ex25-ex26** (Terra e Lua): o primeiro grafo com mais de dois níveis. É
  aquele que mais vezes corre mal; veja a lista de verificação abaixo.
- **ex27** (sistema solar completo): sobretudo repetição, depois de o ex26
  estar a funcionar.
- **ex28** (braço de robot): o mais importante para o Projeto 2. Se tiver
  pouco tempo, salte o ex27 e faça este.

## Lista de verificação

A maior parte dos problemas da Aula 7 resulta de meia dúzia de erros. Antes
de pedir ajuda, confronte o seu código com cada um deles:

1. **Desenhe primeiro o grafo.** Cada nó contém uma transformação, cada aresta
   é uma relação pai-filho. Se o grafo estiver certo, o código é uma tradução
   direta dele; se o código estiver errado, compare-o com o grafo, nó a nó.
2. **Ordem das transformações.** Com a pilha de matrizes, a transformação que
   escreve em *último* lugar é a primeira a ser aplicada aos vértices do
   objeto. Para pôr um planeta em órbita, os seus vértices têm de ser primeiro
   afastados até à distância ao Sol e só depois rodados em torno do Sol: no
   código, `multRotationY` vem antes de `multTranslation`.
3. **`pushMatrix()` e `popMatrix()` andam aos pares.** Faça push antes de cada
   filho (cada ramo do grafo) e pop depois dele. Um pop em falta nota-se em
   astros que herdam transformações que não deviam, como uma Vénus a orbitar
   Mercúrio.
4. **Escale apenas nas folhas.** A escala que dá o tamanho a um astro não pode
   chegar aos seus filhos: aplique-a dentro de um push/pop, imediatamente antes
   de desenhar a primitiva. Caso contrário, a órbita da Lua cresce com o
   diâmetro da Terra.
5. **Rotação de um astro vs. a sua órbita.** Um planeta a rodar em torno do
   próprio eixo e um planeta a orbitar o Sol são duas rotações diferentes, em
   dois níveis diferentes do grafo, com dois períodos diferentes. A Lua deve
   orbitar o centro da Terra, sem acompanhar a rotação da Terra sobre si mesma.
6. **Unidades.** `time` está em dias. Cada ângulo é `360 * time / período`,
   com o período também em dias.
7. **Volume de visão.** Se algo desaparece à medida que acrescenta planetas,
   provavelmente está fora do volume definido pela projeção: ajuste os limites
   (e os planos near/far) à dimensão da cena.

## Para quem já terminou

Estenda o seu braço de robot (ex28):

- Imponha limites a todos os graus de liberdade e garanta que os controlos os
  respeitam.
- Acrescente uma garra com dois dedos que abrem e fecham simetricamente.
- Calcule, em coordenadas do mundo, a posição da ponta da garra (a origem do
  seu sistema de coordenadas) e mostre-a no ecrã. Depois faça o inverso: dado
  um ponto em coordenadas do mundo (por exemplo, o centro de um pequeno cubo
  pousado no chão), determine as suas coordenadas no sistema de coordenadas da
  garra. Ambas as conversões são necessárias no Projeto 2.
