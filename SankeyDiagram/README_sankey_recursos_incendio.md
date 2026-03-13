# Visualização de Fluxo de Recursos para Combate a Incêndios com D3 (Sankey)

# Autores
- João Furtado
- Luiz Oliveira
## Descrição do Projeto

Este projeto implementa uma visualização interativa de fluxo de recursos
utilizando o modelo Sankey com D3.js. O objetivo é representar
graficamente como recursos operacionais utilizados no combate a
incêndios florestais são distribuídos entre diferentes origens e
unidades produtivas (UPs).

O diagrama Sankey permite visualizar o escoamento e a demanda de
recursos, evidenciando quais tipos de recursos estão sendo utilizados e
como eles são alocados entre diferentes unidades.

A visualização serve como uma ferramenta de modelagem e análise para
problemas de alocação de recursos em regiões produtivas, permitindo
compreender melhor a relação entre oferta de recursos e demanda
operacional durante ocorrências de incêndio.

------------------------------------------------------------------------

# Contexto do Problema

Em regiões produtivas florestais, o combate a incêndios depende de
diferentes tipos de recursos operacionais, como aeronaves, brigadistas e
veículos.

Esses recursos:

-   são armazenados por rastreadores
-   podem ser solicitados por diferentes Unidades Produtivas (UPs)
-   podem ser compartilhados entre rastreadores e UPs
-   pertencem a categorias e subcategorias específicas

Uma mesma UP pode demandar recursos provenientes de diferentes
rastreadores, criando um fluxo complexo de alocação de recursos, ideal
para ser representado por um grafo de fluxo Sankey.

------------------------------------------------------------------------

# Estrutura da Visualização

A visualização é organizada em três níveis principais de fluxo:

Rastreadores → Tipo de Recurso → Unidade Produtiva (UP)

Ou conceitualmente:

Origem do recurso → Categoria → Subcategoria → Demanda (UP)

------------------------------------------------------------------------

# Categorias de Recursos

Os recursos são organizados em três grandes grupos principais.

### Aéreo

Recursos utilizados para combate aéreo a incêndios.

Subgrupos:

-   Avião
-   Helicóptero

### Brigadistas

Equipes responsáveis pelo combate direto ao fogo.

Subgrupos:

-   Brigadista Fixo
-   Brigadista Spot

### Veículos

Veículos utilizados para transporte, apoio logístico e combate direto ao
incêndio.

Subgrupos:

-   Veículo Pipa Fixo
-   Veículo Spot
-   Veículo 4x2
-   Veículo Multiuso
-   Veículo 4x4

------------------------------------------------------------------------

# Fluxo de Recursos

O fluxo representado no Sankey segue a seguinte lógica:

1.  **Rastreadores**\
    Dispositivos ou sistemas responsáveis por monitorar a
    disponibilidade e localização dos recursos.

Exemplo:

-   Rastreador A
-   Rastreador B
-   Rastreador C

2.  **Tipo de Recurso**

Os recursos são agrupados nas três categorias principais:

-   Aéreo
-   Brigadista
-   Veículo

3.  **Subtipo do Recurso**

Cada categoria possui subdivisões específicas.

Exemplo:

Aéreo → Avião\
Aéreo → Helicóptero

Brigadista → Fixo\
Brigadista → Spot

Veículo → Pipa Fixo\
Veículo → Spot\
Veículo → 4x2\
Veículo → Multiuso\
Veículo → 4x4

4.  **Unidades Produtivas (UPs)**

As UPs representam as regiões produtivas onde ocorre a demanda por
recursos para combate ao incêndio.

Uma UP pode demandar:

-   múltiplos tipos de recursos
-   recursos de diferentes rastreadores
-   quantidades diferentes de cada recurso

------------------------------------------------------------------------

# Por que usar Sankey?

O Sankey Diagram é ideal para esse tipo de problema porque ele permite
visualizar:

-   fluxo de recursos
-   origem e destino
-   proporção de uso
-   concentração de demanda

A largura das conexões no Sankey representa a quantidade de recursos
sendo transferidos entre os nós.

Isso facilita identificar:

-   gargalos de recursos
-   UPs com maior demanda
-   tipos de recursos mais utilizados
-   dependência de determinados rastreadores

------------------------------------------------------------------------

# Tecnologias Utilizadas

-   D3.js -- biblioteca para visualização de dados
-   D3 Sankey -- extensão para criação de diagramas Sankey
-   JavaScript
-   HTML / CSS

------------------------------------------------------------------------

# Estrutura dos Dados

O Sankey é construído a partir de dois elementos principais:

### Nodes

Representam os elementos do sistema.

Exemplo:

-   Rastreador A
-   Aéreo
-   Helicóptero
-   UP 01

### Links

Representam o fluxo entre os nós.

Exemplo:

Rastreador A → Aéreo\
Aéreo → Helicóptero\
Helicóptero → UP 01

Cada link possui um valor associado, que representa a quantidade de
recursos.

------------------------------------------------------------------------

# Objetivo da Ferramenta

A visualização busca apoiar:

-   análise da distribuição de recursos
-   entendimento da demanda operacional
-   modelagem do problema de alocação
-   tomada de decisão em situações de combate a incêndios

A ferramenta pode auxiliar no planejamento estratégico e na
identificação de padrões de uso de recursos em diferentes regiões
produtivas.

