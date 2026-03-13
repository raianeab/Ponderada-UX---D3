# Modelagem e Visualizações para Alocação de Recursos no Combate a Incêndios

Este documento apresenta visualizações desenvolvidas utilizando
**D3.js** como parte do projeto realizado em parceria com a **Suzano**.

As visualizações foram criadas com o objetivo de **auxiliar na
compreensão do problema de alocação de recursos para combate a incêndios
florestais**, servindo como base conceitual e visual para o
desenvolvimento do sistema do projeto.

Essas representações ajudam a:

-   compreender o fluxo de recursos
-   visualizar relações entre oferta e demanda
-   explorar a distribuição espacial dos recursos

As ferramentas também serão utilizadas como **apoio para apresentação do
projeto e como base para implementação nas próximas sprints**.

Entre as visualizações desenvolvidas estão:

-   modelagem do problema como **fluxo em grafo**
-   visualização de **fluxo de recursos com Sankey**
-   **mapa interativo do Brasil com pontos de monitoramento**

Entre elas, **o mapa interativo será a visualização implementada no
sistema nas próximas sprints do projeto**.

------------------------------------------------------------------------

# Modelagem do Problema como Fluxo em Grafo Bipartido

O problema de alocação de recursos pode ser modelado como um **grafo de
fluxo**, onde diferentes entidades do sistema são representadas como nós
conectados por arestas.

Nesse modelo existem dois conjuntos principais de nós:

-   **Rastreadores (Trackers)** -- representam a origem dos recursos
    disponíveis.
-   **Unidades Produtivas (UPs)** -- representam os locais onde existe
    demanda por recursos.

Para facilitar o cálculo do fluxo, são adicionados dois nós auxiliares:

-   **s (source)** -- origem do fluxo\
-   **t (sink)** -- destino final do fluxo

A estrutura geral do grafo fica:

s → rastreadores → UPs → t

## Capacidades das Arestas

Cada conexão entre os nós possui uma **capacidade**, que representa as
restrições do problema.

-   **s → rastreador**: quantidade de recursos disponíveis naquele
    rastreador.
-   **rastreador → UP**: possibilidade de enviar recursos para
    determinada unidade produtiva.
-   **UP → t**: quantidade de recursos que aquela unidade precisa.

## Execução do Algoritmo

Para encontrar a melhor distribuição de recursos, utiliza-se um
algoritmo de **Minimum Cost Flow**, baseado na ideia do **Successive
Shortest Path**.

O funcionamento ocorre de forma iterativa:

1.  Encontrar o menor caminho entre `s` e `t`
2.  Enviar o máximo fluxo possível por esse caminho
3.  Atualizar as demandas atendidas
4.  Repetir até que todas as demandas sejam satisfeitas

Ao final, o algoritmo identifica **quais rastreadores devem fornecer
recursos para quais unidades produtivas**, minimizando o custo total da
operação.

------------------------------------------------------------------------

# Visualização de Fluxo de Recursos com Sankey

Outra forma de representar o problema é através de um **diagrama
Sankey**, que permite visualizar fluxos entre diferentes elementos de um
sistema.

O Sankey mostra **como os recursos se movem entre origem e destino**,
sendo especialmente útil para problemas de **distribuição e alocação**.

Fluxo representado:

Rastreadores → Tipo de Recurso → Unidade Produtiva

## Estrutura da Visualização

### Rastreadores

Representam os locais onde os recursos estão disponíveis.

Exemplo:

-   Rastreador A\
-   Rastreador B\
-   Rastreador C

### Categorias de Recursos

Os recursos são agrupados em três categorias principais.

**Aéreo**

-   Avião
-   Helicóptero

**Brigadistas**

-   Brigadista Fixo
-   Brigadista Spot

**Veículos**

-   Veículo Pipa Fixo
-   Veículo Spot
-   Veículo 4x2
-   Veículo Multiuso
-   Veículo 4x4

### Unidades Produtivas (UPs)

Representam regiões produtivas onde ocorre a demanda por recursos para
combate ao incêndio.

Uma UP pode demandar:

-   múltiplos tipos de recursos
-   recursos de diferentes rastreadores
-   quantidades variadas de cada recurso

## Por que usar Sankey?

O diagrama Sankey facilita visualizar:

-   fluxo de recursos
-   origem e destino
-   proporção de uso
-   concentração de demanda

A **largura das conexões** representa a quantidade de recursos
transferidos entre os nós, facilitando identificar gargalos e padrões de
uso.

------------------------------------------------------------------------

# Visualização Geográfica com Mapa Interativo do Brasil

Além da modelagem por fluxos, foi criada uma visualização geográfica
utilizando **D3.js**, representando dados diretamente sobre o mapa do
Brasil.

Essa visualização permite observar **onde os recursos e eventos estão
localizados geograficamente**, facilitando a análise espacial do
problema.

O mapa utiliza dados em formato **GeoJSON**, representando:

-   estados do Brasil
-   municípios
-   pontos de monitoramento

## Estrutura do Mapa

O mapa utiliza uma **projeção cartográfica do tipo Mercator**, que
converte latitude e longitude em posições no espaço da visualização.

Elementos organizados em camadas:

Estados → Municípios → Pontos de Monitoramento

### Estados

Divisões administrativas principais do país.

### Municípios

Subdivisões internas dos estados, exibidas apenas em níveis maiores de
zoom.

### Pontos de Monitoramento

Representam locais específicos onde existem dados de interesse.

Cada ponto possui:

-   coordenadas geográficas
-   cor representando nível de alerta
-   tamanho proporcional à intensidade

## Classificação dos Pontos

-   **Vermelho --- Alto**
-   **Laranja --- Médio**
-   **Amarelo --- Baixo**

A legenda no mapa permite interpretar rapidamente os níveis de alerta.

## Interação

O mapa permite:

-   zoom com scroll do mouse
-   botões de zoom
-   navegação interativa

O zoom pode chegar a **50x**, permitindo observar regiões com maior
detalhe.

------------------------------------------------------------------------

# Tecnologias Utilizadas

-   D3.js
-   D3 Sankey
-   GeoJSON
-   SVG
-   HTML / CSS / JavaScript

------------------------------------------------------------------------

# Papel das Visualizações no Projeto

Essas visualizações foram desenvolvidas como **ferramentas exploratórias
para modelagem do problema** dentro do projeto com a **Suzano**.

Elas auxiliam na:

-   compreensão do fluxo de recursos
-   análise da distribuição de demandas
-   visualização espacial das ocorrências
-   comunicação do funcionamento do sistema

Entre as visualizações desenvolvidas, **o mapa interativo do Brasil será
a base para implementação no sistema nas próximas sprints**.

------------------------------------------------------------------------

# Autores

## Modelagem de Fluxo

-   Bruno Kadayan
-   Antonio Andre

## Visualização Sankey

-   João Furtado
-   Luiz Oliveira

## Visualização do Mapa

-   Anne Figueirôa
-   Raiane Brandão