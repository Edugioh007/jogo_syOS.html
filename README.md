# jogo_syOS.html
jogo de 3 pistas secretas
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Jogo de Palavras Secretas - SyOS</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f4f4;
      text-align: center;
      padding: 40px;
    }
    .game-container {
      background: white;
      max-width: 600px;
      margin: auto;
      padding: 30px;
      border-radius: 10px;
      box-shadow: 0 0 10px #ccc;
    }
    .pistas {
      margin: 20px 0;
    }
    input, button {
      padding: 10px;
      font-size: 16px;
      margin: 5px;
    }
    .correct {
      color: green;
      font-weight: bold;
    }
    .wrong {
      color: red;
      font-weight: bold;
    }
    #ranking {
      margin-top: 30px;
      text-align: left;
    }
  </style>
</head>
<body>
  <div class="game-container">
    <h1>🔍 Jogo: Descubra a Palavra Secreta</h1>
    <div id="rodada"></div>
    <div class="pistas" id="pistas"></div>
    <input type="text" id="resposta" placeholder="Digite sua resposta aqui">
    <button onclick="verificarResposta()">Verificar</button>
    <p id="feedback"></p>
    <div id="final" style="display: none;">
      <h2>Pontuação Final: <span id="pontuacao"></span></h2>
      <input type="text" id="nomeJogador" placeholder="Seu nome">
      <button onclick="salvarRanking()">Salvar no Ranking</button>
    </div>
    <div id="ranking" style="display: none;">
      <h3>🏆 Ranking:</h3>
      <ol id="listaRanking"></ol>
    </div>
  </div>

  <script>
    const rodadas = [
      {
        resposta: "workflow",
        pistas: [
          "É usado para automatizar processos manuais.",
          "Ajuda a reduzir erros no controle de estoque.",
          "Pode ser criado com Google Forms e Sheets."
        ]
      },
      {
        resposta: "embalagem",
        pistas: [
          "Afeta diretamente a percepção do cliente.",
          "Pode ser personalizada com logo e cores.",
          "Foi apontada como um problema na SyOS."
        ]
      },
      {
        resposta: "kpi",
        pistas: [
          "Serve para medir o desempenho dos processos.",
          "Exemplos: precisão do estoque, tempo de localização.",
          "É uma sigla em inglês."
        ]
      },
      {
        resposta: "qr code",
        pistas: [
          "Facilita a identificação rápida de produtos.",
          "Pode ser colocado nas caixas do estoque.",
          "Evita duplicidade de registros."
        ]
      },
      {
        resposta: "auditoria",
        pistas: [
          "É feita para comparar o estoque real com o digital.",
          "É uma verificação rotineira dos processos.",
          "Garante maior confiabilidade nos dados."
        ]
      }
    ];

    let rodadaAtual = 0;
    let pontos = 0;

    function carregarRodada() {
      document.getElementById("rodada").innerText = `Rodada ${rodadaAtual + 1}`;
      const pistasHTML = rodadas[rodadaAtual].pistas.map(p => `<li>${p}</li>`).join('');
      document.getElementById("pistas").innerHTML = `<ul>${pistasHTML}</ul>`;
      document.getElementById("resposta").value = "";
      document.getElementById("feedback").innerText = "";
    }

    function verificarResposta() {
      const resposta = document.getElementById("resposta").value.trim().toLowerCase();
      const correta = rodadas[rodadaAtual].resposta.toLowerCase();

      if (resposta === correta) {
        document.getElementById("feedback").innerHTML = `<span class="correct">✅ Correto!</span>`;
        pontos++;
        rodadaAtual++;
        if (rodadaAtual < rodadas.length) {
          setTimeout(() => carregarRodada(), 1000);
        } else {
          document.getElementById("rodada").style.display = "none";
          document.getElementById("pistas").innerHTML = "";
          document.getElementById("resposta").style.display = "none";
          document.querySelector("button").style.display = "none";
          document.getElementById("feedback").innerHTML = "";
          document.getElementById("final").style.display = "block";
          document.getElementById("pontuacao").innerText = pontos;
        }
      } else {
        document.getElementById("feedback").innerHTML = `<span class="wrong">❌ Tente novamente!</span>`;
      }
    }

    function salvarRanking() {
      const nome = document.getElementById("nomeJogador").value.trim();
      if (!nome) return alert("Digite um nome!");

      const ranking = JSON.parse(localStorage.getItem("rankingSyOS") || "[]");
      ranking.push({ nome, pontos });
      ranking.sort((a, b) => b.pontos - a.pontos);
      ranking.splice(10); // mostra apenas top 10
      localStorage.setItem("rankingSyOS", JSON.stringify(ranking));

      exibirRanking();
    }

    function exibirRanking() {
      document.getElementById("ranking").style.display = "block";
      const ranking = JSON.parse(localStorage.getItem("rankingSyOS") || "[]");
      const lista = document.getElementById("listaRanking");
      lista.innerHTML = ranking.map(r => `<li>${r.nome}: ${r.pontos} ponto(s)</li>`).join('');
    }

    carregarRodada();
  </script>
</body>
</html>

