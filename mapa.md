<script>
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

</script>
