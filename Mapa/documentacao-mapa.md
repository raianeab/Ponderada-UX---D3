## **Visualização Geográfica do Monitoramento no Brasil**

Para apoiar a análise espacial do monitoramento, foi construída uma visualização interativa utilizando **D3.js**, permitindo representar dados geográficos sobre o mapa do Brasil.

A visualização utiliza dados cartográficos em formato **GeoJSON**, representando:

- **Estados do Brasil**
- **Municípios**
- **Pontos de monitoramento**

Esses elementos são renderizados em camadas SVG, permitindo interação e manipulação dinâmica do mapa.

A estrutura visual é composta por três camadas principais:

Estados → Municípios → Pontos de Monitoramento

Cada camada possui uma função específica na representação da informação.

---

## **Estrutura do Mapa**

O mapa é construído utilizando uma **projeção geográfica do tipo Mercator**, que converte coordenadas de latitude e longitude em posições dentro do espaço SVG.

Essa projeção permite posicionar corretamente os elementos geográficos do Brasil no plano bidimensional da visualização.

Os elementos do mapa são organizados da seguinte forma:

- **Estados**  
  Representam as divisões administrativas principais do país.  
  São desenhados com bordas mais destacadas para facilitar a identificação territorial.

- **Municípios**  
  Representam subdivisões internas dos estados.  
  Essas divisões aparecem apenas em níveis maiores de zoom para evitar sobrecarga visual.

- **Pontos de Monitoramento**  
  Representam locais específicos onde há coleta ou análise de dados.

Cada ponto possui:

- nome da localidade  
- coordenadas geográficas (latitude e longitude)  
- cor representando o nível de monitoramento  
- tamanho proporcional ao nível de alerta

---

## **Classificação dos Pontos**

Os pontos de monitoramento são classificados em três níveis de intensidade, representados visualmente por cores distintas.

- **Vermelho — Alto**  
  Representa locais com alto nível de alerta ou monitoramento.

- **Laranja — Médio**  
  Indica uma condição intermediária.

- **Amarelo — Baixo**  
  Representa níveis baixos de monitoramento.

Essa categorização é reforçada por uma **legenda visual presente no mapa**, permitindo interpretação rápida dos dados.

---

## **Interação e Navegação**

Para permitir exploração detalhada do mapa, foi implementado um sistema de **zoom e navegação** utilizando o módulo `d3.zoom`.

O usuário pode interagir de duas formas:

- **Scroll do mouse**  
- **Botões de zoom (+ e -)**

O sistema permite ampliar o mapa até **50 vezes**, facilitando a visualização de regiões específicas.

Durante a navegação, alguns comportamentos dinâmicos são aplicados:

- As **divisões municipais aparecem apenas quando o nível de zoom é maior que 3**, evitando poluição visual quando o mapa está distante.
- A **espessura das bordas dos estados e municípios é ajustada dinamicamente**, garantindo que elas permaneçam visíveis independentemente do nível de zoom.
- O **tamanho dos pontos de monitoramento é reduzido proporcionalmente ao zoom**, impedindo que eles fiquem excessivamente grandes quando o mapa é ampliado.

---

## **Carregamento Progressivo dos Dados**

Para melhorar a experiência do usuário, os dados geográficos são carregados de forma progressiva.

O processo ocorre em duas etapas:

1. **Carregamento das fronteiras dos estados**
2. **Carregamento das divisões municipais**

Durante o processo, um indicador visual informa ao usuário que os dados ainda estão sendo carregados.

Após o carregamento completo, o indicador é removido e o mapa torna-se totalmente interativo.

---

## **Representação dos Pontos no Mapa**

Os pontos de monitoramento são desenhados utilizando elementos **SVG do tipo círculo**.

Cada ponto é posicionado a partir de suas coordenadas geográficas, convertidas pela projeção cartográfica.

Além disso, cada ponto possui um **tooltip**, exibido ao passar o cursor sobre ele, contendo o nome da localidade e sua classificação.

Essa funcionalidade facilita a identificação rápida dos locais monitorados.

---

## **Resultado**

A visualização permite explorar espacialmente os pontos de monitoramento distribuídos pelo território brasileiro, oferecendo:

- navegação interativa com zoom e deslocamento
- identificação visual dos níveis de alerta
- detalhamento progressivo das divisões territoriais
- localização precisa dos pontos monitorados

Dessa forma, o mapa funciona como uma ferramenta de apoio para compreender a distribuição geográfica dos dados analisados.

---

## **Tecnologias Utilizadas**

- **D3.js** — biblioteca JavaScript para manipulação de documentos baseados em dados.
- **GeoJSON** — formato utilizado para representar dados geográficos.
- **SVG** — utilizado para renderização dos elementos visuais do mapa.

---

## **Autores**

- Anne Figueirôa  
- Raiane Brandão
