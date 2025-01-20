<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ficha de Frequência</title>
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap" rel="stylesheet">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.17.0/xlsx.full.min.js"></script>
    <style>
        body {
            font-family: 'Roboto', sans-serif;
            margin: 0;
            padding: 0;
            color: #fff;
            background-color: #1a1a2e; /* Fundo escuro */
            background-image: linear-gradient(135deg, #1a1a2e, #16213e);
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        .container {
            max-width: 600px;
            margin: auto;
            padding: 100px;
            border-radius: 10px;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.5);
            background-color: rgba(255, 255, 255, 0.1); /* Fundo semi-transparente */
            backdrop-filter: blur(10px); /* Efeito de desfoque */
        }
        .logo {
            text-align: center;
            margin-bottom: 20px;
        }
        .logo img {
            max-width: 150px;
            height: auto;
            border-radius: 10px;
        }
        h1 {
            text-align: center;
            color: #00adb5; /* Azul claro */
            font-size: 28px;
            margin-bottom: 20px;
            text-shadow: 1px 1px 5px rgba(0, 0, 0, 0.5);
        }
        label {
            display: block;
            margin: 10px 0 5px;
            color: #f8f8f2; /* Cor clara */
            font-weight: 500;
        }
        select,
        input[type="text"],
        input[type="date"],
        input[type="time"] {
            width: 100%;
            padding: 12px;
            border: 2px solid #00adb5; /* Azul claro */
            border-radius: 5px;
            margin-bottom: 15px;
            background-color: #1f1f2e; /* Fundo escuro */
            color: #fff;
        }
        select:focus,
        input[type="text"]:focus,
        input[type="date"]:focus,
        input[type="time"]:focus {
            border-color: #f8f8f2; /* Cor clara */
            outline: none;
            background-color: #2a2a3e; /* Fundo um pouco mais claro */
        }
        button {
            background-color: #00adb5; /* Azul claro */
            color: white;
            padding: 12px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            width: 100%;
            font-size: 16px;
            transition: background-color 0.3s ease;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
        }
        button:hover {
            background-color: #009a9a; /* Azul mais escuro */
        }
        .output {
            margin-top: 20px;
            background-color: rgba(255, 255, 255, 0.05); /* Fundo semi-transparente */
            padding: 15px;
            border-radius: 5px;
            max-height: 200px;
            overflow-y: auto;
            color: #f8f8f2; /* Cor clara */
        }
        .output p {
            margin: 5px 0;
            font-size: 14px;
            line-height: 1.5;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="logo">
            <img src="caminho/para/sua/logo.png" alt="Logo da Cooperativa"> <!-- Substitua pelo caminho da sua logomarca -->
        </div>
        <h1>Ficha de Frequência</h1>
        
        <label for="cooperado">Nome do Cooperado:</label>
        <select id="cooperado" onchange="carregarInformacoes()">
            <option value="">Selecione um cooperado</option>
            <!-- Os nomes dos cooperados serão preenchidos aqui -->
        </select>

        <label for="funcao">Função:</label>
        <input type="text" id="funcao" readonly>

        <label for="setor">Setor:</label>
        <input type="text" id="setor" readonly>

        <label for="matricula">Matrícula:</label>
        <input type="text" id="matricula" readonly>

        <label for="coren">Número do COREN:</label>
        <input type="text" id="coren" readonly>

        <label for="data">Data:</label>
        <input type="date" id="data" required>

        <label for="horaEntrada">Hora de Entrada:</label>
        <input type="time" id="horaEntrada" required>

        <label for="horaSaida">Hora de Saída:</label>
        <input type="time" id="horaSaida" required>

        <button onclick="registrarFrequencia()">Registrar Frequência</button>

        <div class="output" id="output"></div>
    </div>

    <script>
        // Carregar dados da planilha
        async function carregarDados() {
            const response = await fetch('caminho/para/sua/planilha/cooperados.xlsx'); // Substitua pelo caminho da sua planilha
            const data = await response.arrayBuffer();
            const workbook = XLSX.read(data);
            const sheetName = workbook.SheetNames[0];
            const sheet = workbook.Sheets[sheetName];
            const jsonData = XLSX.utils.sheet_to_json(sheet);

            // Preencher o dropdown com os nomes dos cooperados
            const cooperadoSelect = document.getElementById('cooperado');
            jsonData.forEach(cooperado => {
                const option = document.createElement('option');
                option.value = cooperado.Nome; // Certifique-se de que a coluna se chama "Nome"
                option.textContent = cooperado.Nome;
                cooperadoSelect.appendChild(option);
            });
        }

        // Carregar informações do cooperado selecionado
        function carregarInformacoes() {
            const cooperadoSelect = document.getElementById('cooperado');
            const selectedName = cooperadoSelect.value;

            // Encontrar o cooperado correspondente
            const cooperadoData = jsonData.find(cooperado => cooperado.Nome === selectedName);
            if (cooperadoData) {
                document.getElementById('funcao').value = cooperadoData.Função; // Certifique-se de que a coluna se chama "Função"
                document.getElementById('setor').value = cooperadoData.Setor; // Certifique-se de que a coluna se chama "Setor"
                document.getElementById('matricula').value = cooperadoData.Matrícula; // Certifique-se de que a coluna se chama "Matrícula"
                document.getElementById('coren').value = cooperadoData['Número do COREN']; // Certifique-se de que a coluna se chama "Número do COREN"
            }
        }

        // Função para registrar a frequência
        function registrarFrequencia() {
            const nome = document.getElementById('cooperado').value;
            const dataInput = document.getElementById('data').value;
            const horaEntrada = document.getElementById('horaEntrada').value;
            const horaSaida = document.getElementById('horaSaida').value;

            if (nome && dataInput && horaEntrada && horaSaida) {
                // Formatar a data para dia/mês/ano
                const dataParts = dataInput.split('-');
                const dataFormatada = `${dataParts[2]}/${dataParts[1]}/${dataParts[0]}`;

                const output = document.getElementById('output');
                output.innerHTML += `<p><strong>Nome:</strong> ${nome} | <strong>Data:</strong> ${dataFormatada} | <strong>Entrada:</strong> ${horaEntrada} | <strong>Saída:</strong> ${horaSaida}</p>`;
                
                // Limpar os campos após o registro
                document.getElementById('data').value = '';
                document.getElementById('horaEntrada').value = '';
                document.getElementById('horaSaida').value = '';
            } else {
                alert('Por favor, preencha todos os campos.');
            }
        }

        // Carregar os dados da planilha ao carregar a página
        window.onload = carregarDados;
    </script>
</body>
</html>
