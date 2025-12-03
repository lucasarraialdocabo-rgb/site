# site<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Navegar com Consciência – Cadastro</title>

<style>
    body {
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        background: #e8f6ff;
        margin: 0;
        padding: 0;
    }

    header {
        background: #005f8c;
        color: white;
        padding: 15px;
        text-align: center;
        font-size: 22px;
        font-weight: bold;
        box-shadow: 0 2px 5px rgba(0,0,0,0.2);
    }

    /* --- NOVO ESTILO DO BANNER COM IMAGEM --- */
    .hero-banner {
        /* Aqui definimos a camada escura E a imagem de fundo */
        background: linear-gradient(rgba(0, 47, 75, 0.6), rgba(0, 47, 75, 0.6)),
                    url('https://images.unsplash.com/photo-1518837695005-2083093ee35b?q=80&w=1600&auto=format&fit=crop');
        
        /* Ajustes para a imagem cobrir tudo e ficar centralizada */
        background-size: cover;
        background-position: center;
        background-repeat: no-repeat;

        color: white;
        padding: 60px 20px; /* Aumentei um pouco o tamanho vertical */
        text-align: center;
        margin-bottom: 20px;
        /* Sombra no texto para facilitar a leitura sobre a foto */
        text-shadow: 1px 1px 4px rgba(0,0,0,0.7);
    }

    .hero-banner h1 {
        margin: 0 0 10px 0;
        font-size: 32px; /* Título um pouco maior */
    }

    .hero-banner p {
        font-size: 18px;
        margin-bottom: 25px;
        max-width: 800px;
        margin-left: auto;
        margin-right: auto;
        font-weight: 500;
    }

    .tips-container {
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
        gap: 15px;
    }

    .tip-card {
        /* Fundo mais transparente para combinar com a foto */
        background: rgba(255, 255, 255, 0.15);
        backdrop-filter: blur(5px);
        padding: 10px 20px;
        border-radius: 20px;
        font-weight: bold;
        border: 1px solid rgba(255,255,255,0.3);
        display: flex;
        align-items: center;
        gap: 8px;
        text-shadow: none; /* Remove a sombra dos cards pequenos */
    }
    /* ----------------------------------------------- */

    .container {
        max-width: 700px;
        margin: 0 auto 40px auto;
        background: white;
        padding: 20px;
        border-radius: 10px;
        box-shadow: 0 2px 8px rgba(0,0,0,0.15);
    }

    h2 { color: #0077b6; }

    label {
        font-weight: bold;
        margin-top: 15px;
        display: block;
        color: #333;
    }

    input, textarea, select {
        width: 100%;
        padding: 12px;
        margin-top: 5px;
        border-radius: 6px;
        border: 1px solid #ccc;
        box-sizing: border-box;
    }

    button {
        margin-top: 20px;
        padding: 12px;
        width: 100%;
        background: #0077b6;
        border: none;
        border-radius: 6px;
        color: white;
        font-size: 16px;
        font-weight: bold;
        cursor: pointer;
        transition: background 0.3s;
    }

    button:hover {
        background: #023e8a;
    }

    .list {
        margin-top: 40px;
        border-top: 2px solid #e8f6ff;
        padding-top: 20px;
    }

    .card {
        background: #f1faff;
        border-left: 5px solid #0077b6;
        padding: 15px;
        border-radius: 6px;
        margin-bottom: 15px;
        box-shadow: 0 1px 3px rgba(0,0,0,0.1);
    }

    .deleteBtn {
        background: #d62828;
        margin-top: 10px;
        width: auto;
        padding: 8px 15px;
        font-size: 14px;
    }
    .deleteBtn:hover {
        background: #a81818;
    }
</style>
</head>

<body>

<header>Navegar com Consciência</header>

<div class="hero-banner">
    <h1>Proteja Nossos Oceanos</h1>
    <p>O mar é o nosso maior patrimônio. Ao registrar uma ocorrência, você ajuda a monitorar e cuidar da vida marinha.</p>
    
    <div class="tips-container">
        <div class="tip-card">🚫 Não jogue lixo ao mar</div>
        <div class="tip-card">🐢 Respeite a fauna</div>
        <div class="tip-card">⚓ Ancore com cuidado</div>
        <div class="tip-card">🧴 Use protetor biodegradável</div>
    </div>
</div>

<div class="container">

    <h2>Cadastrar Nova Ocorrência</h2>

    <label>Título</label>
    <input id="titulo" placeholder="Ex: Rede perdida, animal avistado, mancha de óleo..." />

    <label>Categoria</label>
    <select id="categoria">
        <option>Observação de Fauna</option>
        <option>Perigo / Poluição</option>
        <option>Ação de Limpeza</option>
        <option>Conscientização</option>
    </select>

    <label>Descrição</label>
    <textarea id="descricao" placeholder="Descreva o ocorrido com detalhes..." rows="4"></textarea>

    <label>Localização (GPS)</label>
    <input id="coords" readonly placeholder="Capturando localização..." style="background-color: #f0f0f0;" />

    <button onclick="salvar()">Salvar Ocorrência</button>

    <div class="list">
        <h2>Ocorrências Registradas</h2>
        <div id="lista"></div>
    </div>

</div>

<script>
    // Captura automática da localização
    function capturarLocalizacao() {
        if (!navigator.geolocation) {
            document.getElementById("coords").value = "Geolocalização não suportada.";
            return;
        }

        navigator.geolocation.getCurrentPosition(
            pos => {
                const lat = pos.coords.latitude.toFixed(6);
                const lon = pos.coords.longitude.toFixed(6);
                document.getElementById("coords").value = lat + ", " + lon;
            },
            err => {
                console.error(err);
                document.getElementById("coords").value = "Permissão de local negada.";
            },
            {
                enableHighAccuracy: true
            }
        );
    }
    capturarLocalizacao();

    // Salvar no LocalStorage
    function salvar() {
        const titulo = document.getElementById("titulo").value.trim();
        const categoria = document.getElementById("categoria").value;
        const descricao = document.getElementById("descricao").value.trim();
        const coords = document.getElementById("coords").value;

        if (!titulo) return alert("Por favor, preencha o título da ocorrência.");

        const registro = {
            id: Date.now(),
            titulo,
            categoria,
            descricao,
            coords,
            data: new Date().toLocaleString('pt-BR')
        };

        const lista = JSON.parse(localStorage.getItem("ocorrencias")) || [];
        lista.unshift(registro);
        localStorage.setItem("ocorrencias", JSON.stringify(lista));

        limpar();
        mostrarLista();
        alert("Ocorrência registrada com sucesso!");
    }

    function limpar() {
        document.getElementById("titulo").value = "";
        document.getElementById("descricao").value = "";
    }

    function excluir(id) {
        if (!confirm("Tem certeza que deseja excluir esta ocorrência?")) return;

        let lista = JSON.parse(localStorage.getItem("ocorrencias")) || [];
        lista = lista.filter(item => item.id !== id);
        localStorage.setItem("ocorrencias", JSON.stringify(lista));
        mostrarLista();
    }

    function mostrarLista() {
        const lista = JSON.parse(localStorage.getItem("ocorrencias")) || [];
        const div = document.getElementById("lista");
        div.innerHTML = "";

        if (lista.length === 0) {
            div.innerHTML = "<p style='color: #666; font-style: italic;'>Nenhuma ocorrência registrada ainda.</p>";
            return;
        }

        lista.forEach(item => {
            const card = document.createElement("div");
            card.className = "card";

            card.innerHTML = `
                <div style="display:flex; justify-content:space-between; align-items:flex-start;">
                    <div>
                        <strong style="font-size:1.1em; color:#005f8c;">${item.titulo}</strong>
                        <br>
                        <span style="background:#e0f7fa; color:#006064; padding:2px 8px; border-radius:10px; font-size:0.8em;">${item.categoria}</span>
                    </div>
                    <small style="color:#888;">${item.data}</small>
                </div>
                <p style="margin: 10px 0;">${item.descricao}</p>
                <div style="background: white; padding: 5px; border-radius: 4px; border: 1px dashed #ccc; display:inline-block;">
                    <small>📍 <b>Local:</b> ${item.coords}</small>
                </div>
                <br>
                <button class="deleteBtn" onclick="excluir(${item.id})">Excluir</button>
            `;

            div.appendChild(card);
        });
    }

    mostrarLista();
</script>

</body>
</html>
