```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <title>Mapa Brasil - Zoom Ampliado e Legenda</title>
    <script src="https://d3js.org/d3.v7.min.js"></script>
    <script src="https://unpkg.com/topojson-client@3"></script>
    <style>
        body { font-family: sans-serif; display: flex; flex-direction: column; align-items: center; background: #F0F0F0; margin: 0; }
        #container { position: relative; margin-top: 20px; }
        #mapa { background: #AADAFF; border-radius: 8px; box-shadow: 0 4px 10px rgba(0,0,0,0.2); overflow: hidden; }

        /* Municípios: Linhas visíveis */
        .municipio { fill: #F9F9F9; stroke: #999; stroke-width: 0.2px; }

        /* Estados: Borda bem definida */
        .estado { fill: rgba(220, 220, 220, 0.3); stroke: #333; stroke-width: 1px; pointer-events: none; }

        .ponto { stroke: #000; stroke-width: 0.5px; opacity: 1; transition: r 0.2s; }

        /* Controles de Zoom */
        .controls { position: absolute; top: 15px; left: 15px; display: flex; flex-direction: column; gap: 8px; z-index: 10; }
        button { width: 40px; height: 40px; cursor: pointer; border: 1px solid #999; background: white; font-size: 1.2em; border-radius: 4px; box-shadow: 0 2px 4px rgba(0,0,0,0.1); }
        button:hover { background: #eee; }

        /* Loading */
        #loading { position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); background: white; padding: 10px; border-radius: 5px; box-shadow: 0 2px 5px rgba(0,0,0,0.2); z-index: 20; }

        /* Estilos da Legenda */
        .legenda {
            position: absolute;
            bottom: 20px;
            right: 20px;
            background: white;
            padding: 10px;
            border-radius: 8px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
            z-index: 10;
            pointer-events: none; /* Impede que a legenda bloqueie interações com o mapa abaixo */
        }
        .legenda-item {
            display: flex;
            align-items: center;
            margin-bottom: 5px;
        }
        .legenda-item:last-child {
            margin-bottom: 0;
        }
        .legenda-cor {
            width: 15px;
            height: 15px;
            border-radius: 50%;
            border: 1px solid #000;
            margin-right: 10px;
        }
        .legenda-texto {
            font-size: 14px;
            color: #333;
        }
    </style>
</head>
<body>

    <h2>Monitoramento Brasil (Use o scroll ou botões para zoom)</h2>

    <div id="container">
        <div id="loading">Carregando mapa...</div>
        <div class="controls">
            <button id="zoom_in">+</button>
            <button id="zoom_out">-</button>
        </div>

        <div class="legenda">
            <div class="legenda-item">
                <div class="legenda-cor" style="background: red;"></div>
                <div class="legenda-texto">Alto</div>
            </div>
            <div class="legenda-item">
                <div class="legenda-cor" style="background: orange;"></div>
                <div class="legenda-texto">Médio</div>
            </div>
            <div class="legenda-item">
                <div class="legenda-cor" style="background: yellow;"></div>
                <div class="legenda-texto">Baixo</div>
            </div>
        </div>

        <div id="mapa"></div>
    </div>

    <script>
        const width = 700;
        const height = 700;

        const svg = d3.select("#mapa").append("svg")
            .attr("width", width)
            .attr("height", height);

        const g = svg.append("g");
        const camadaMunicipios = g.append("g");
        const camadaEstados = g.append("g");
        const camadaPontos = g.append("g");

        const projection = d3.geoMercator()
            .center([-55, -15])
            .scale(950)
            .translate([width / 2, height / 2]);

        const path = d3.geoPath().projection(projection);

        const dadosPontos = [
            { nome: "São Paulo - Alto", lon: -46.63, lat: -23.55, cor: "red", tamanho: 12 },
            { nome: "Brasília - Médio", lon: -47.88, lat: -15.79, cor: "orange", tamanho: 8 },
            { nome: "Manaus - Baixo", lon: -60.02, lat: -3.11, cor: "yellow", tamanho: 5 },
            { nome: "Salvador - Alto", lon: -38.50, lat: -12.97, cor: "red", tamanho: 12 }
        ];

        // Configuração do Zoom - Aumentado para 50x
        const zoomBehavior = d3.zoom()
            .scaleExtent([1, 50]) // Limite de zoom aumentado significativamente
            .on("zoom", (event) => {
                const { k } = event.transform;
                g.attr("transform", event.transform);

                // Municípios aparecem a partir do zoom 3
                camadaMunicipios.style("opacity", k > 3 ? 1 : 0);

                // Ajuste dinâmico das bordas para manter visibilidade sem engrossar
                camadaEstados.selectAll("path").style("stroke-width", 1 / k);
                camadaMunicipios.selectAll("path").style("stroke-width", 0.3 / k);
                camadaPontos.selectAll("circle").attr("r", d => d.tamanho / k);
            });

        svg.call(zoomBehavior);
        d3.select("#zoom_in").on("click", () => svg.transition().duration(500).call(zoomBehavior.scaleBy, 2));
        d3.select("#zoom_out").on("click", () => svg.transition().duration(500).call(zoomBehavior.scaleBy, 0.5));

        // Carregar Estados
        d3.json("https://raw.githubusercontent.com/codeforamerica/click_that_hood/master/public/data/brazil-states.geojson").then(estados => {
            camadaEstados.selectAll("path")
                .data(estados.features)
                .join("path")
                .attr("class", "estado")
                .attr("d", path);

            d3.select("#loading").text("Carregando divisões municipais...");

            // Carregar Municípios
            d3.json("https://raw.githubusercontent.com/tbrugz/geodata-br/master/geojson/geojs-100-mun.json").then(mun => {
                d3.select("#loading").remove();

                camadaMunicipios.selectAll("path")
                    .data(mun.features)
                    .join("path")
                    .attr("class", "municipio")
                    .attr("d", path)
                    .append("title")
                    .text(d => d.properties.name || d.properties.description);

                camadaMunicipios.style("opacity", 0);
            });
        });

        // Desenhar Pontos
        camadaPontos.selectAll("circle")
            .data(dadosPontos)
            .join("circle")
            .attr("class", "ponto")
            .attr("cx", d => projection([d.lon, d.lat])[0])
            .attr("cy", d => projection([d.lon, d.lat])[1])
            .attr("r", d => d.tamanho)
            .attr("fill", d => d.cor)
            .append("title")
            .text(d => d.nome);

    </script>
</body>
</html>
```


#Estrutura básica do HTML
- O documento é em HTML5
- O lang="pt-br" indica que a página está em português brasileiro

#Cabeçalho (head)
- charset="UTF-8" permite acentuação
- title nomeia a aba

#Bibliotecas externas
- Importa as bibliotecas d3(desenha mapas, cria gráficos e manipula svg) e TopoJSON(leitura de mapas compactados)

#CSS (estilo visual)
- body centraliza tudo, usa fonte simples e define o fundo como cinza
- "#mapa" define o fundo do mapa como azul claro para simular o oceano
- .municipio preenche os municípios com a cor branca e cria linhas finas cinzas
- .estado escurece as bordas e divide os estados
- .ponto cria círculos que representam o monitoramento
- .controls define a posição dos botões + e -

#body (corpo da página)
- Define o título como "Monitoramento Brasil"
- div guarda loading, botões, legenda e mapa

#Tela de carregamento
- Exibe mensagem enquanto os dados do mapa são baixados

#Botões
- Aproxima e afasta o mapa

#Legenda
- Mostra a lengenda das cores do mapa (vermelho = alto, laranja = médio, amarelo = baixo)

#Área do mapa
- Cria o mapa SVG a partir do d3

#Javascript
- width e height definem as dimensões do mapa
- SVG é usado para desenhar mapas e gráficos
- As 3 camadas dividem o mapa por cidades, estados e eventos
- projection transforma x e y em longitude e latitude
- path converte os dados geográficos em desenhos SVG
- dadosPontos define as posições dos círculos
- zoomBehavior permite zoom até 50x com scroll e arrastar o mapa
- A camada dos municípios aparecem quando o zoom é maior que 3
- stroke-width evita que as linhas fiquem grossas no zoom
- svg.call ativa o zoom
- d3.select faz os botões controlarem o zoom
- d3.json baixa o mapa em GeoJSON
- camadaEstados.selectAll("path") cria paths SVG para cada estado
- camadaMunicipios.selectAll("path") transforma cada município em path no SVG
- .append("title") e .text(...) mostra textos ao passar o mouse por cima
- camadaPontos.selectAll("circle") cria os círculos no mapa
- .attr("cx", d => projection([d.lon, d.lat])[0]) e .attr("cy", d => projection([d.lon, d.lat])[1]) transforma longitude e latitude em x e y
