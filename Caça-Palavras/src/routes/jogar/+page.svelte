<script>
  import "../../styles/jogar.css";

  const palavras = [
    "PROGRAMAÇÃO",
    "CODIFICAÇÃO",
    "ALGORITMOS",
    "VARIÁVEIS",
    "FUNÇÕES",
    "LÓGICA",
    "JAVASCRIPT",
    "BANCODEDADOS",
    "ARRAY",
    "COMPILAÇÃO",
    "CSHARP",
    "PYTHON",
    "PROGRAMADOR",
    "SQL",
    "RESPONSIVO",
    "RUBY",
  ];

  let letras = [
    ["P", "R", "O", "G", "R", "A", "M", "A", "Ç", "Ã", "O", "L"],
    ["C", "O", "D", "I", "F", "I", "C", "A", "Ç", "Ã", "O", "M"],
    ["A", "L", "G", "O", "R", "I", "T", "M", "O", "S", "P", "C"],
    ["V", "A", "R", "I", "Á", "V", "E", "I", "S", "D", "Ç", "S"],
    ["F", "U", "N", "Ç", "Õ", "E", "S", "R", "C", "D", "E", "Q"],
    ["L", "Ó", "G", "I", "C", "A", "P", "R", "O", "G", "R", "L"],
    ["J", "A", "V", "A", "S", "C", "R", "I", "P", "T", "I", "O"],
    ["B", "A", "N", "C", "O", "D", "E", "D", "A", "D", "O", "S"],
    ["A", "R", "R", "A", "Y", "O", "P", "Y", "T", "H", "O", "N"],
    ["C", "O", "M", "P", "I", "L", "A", "Ç", "Ã", "O", "A", "L"],
    ["C", "S", "H", "A", "R", "P", "C", "L", "A", "S", "S", "E"],
    ["R", "U", "B", "Y", "O", "N", "D", "E", "F", "O", "R", "T"],
    ["P", "R", "O", "G", "R", "A", "M", "A", "D", "O", "R", "E"],
    ["S", "V", "L", "A", "L", "Z", "O", "W", "I", "T", "M", "O"],
    ["R", "E", "S", "P", "O", "N", "S", "I", "V", "O", "D", "A"],
  ];

  let contador = palavras.length;

  let selecionando = false;
  let celulasSelecionadas = [];
  let textoSelecionado = "";

  function selecionarCelula(event, i, j) {
    const cell = event.target;
    const rowIndex = i;
    const cellIndex = j;

    if (event.type === "mousedown") {
      // Inicia a seleção apenas quando o botão do mouse é pressionado
      selecionando = true;
      celulasSelecionadas = [{ linha: rowIndex, coluna: cellIndex }];
    } else if (event.type === "mouseenter" && selecionando) {
      // Adiciona células à seleção apenas quando o mouse está pressionado e movendo
      celulasSelecionadas.push({ linha: rowIndex, coluna: cellIndex });
    }

    if (selecionando) {
      const palavraFormada = obterPalavraFormada();
      console.log("Palavra formada:", palavraFormada);

      cell.classList.toggle("selecionada");

      compararPalavra(palavraFormada);
      textoSelecionado = obterPalavraFormada();
    }

    if (event.type === "mouseup") {
      // Finaliza a seleção quando o botão do mouse é liberado
      selecionando = false;
    }
  }

  function resetPalavra() {
    celulasSelecionadas = [];
    selecionando = false;
    textoSelecionado = "";
  }

  function apagarConteudoSelecionado() {
    celulasSelecionadas.forEach(({ linha, coluna }) => {
      const cell = document.querySelector(`.cell-${linha}-${coluna}`);
      if (cell) {
        cell.classList.remove("selecionada");
      }
    });
    resetPalavra();
  }

  function exibirMensagemParabens() {
    alert("Parabéns! Você encontrou todas as palavras!");
  }

  function compararPalavra(palavraFormada) {
    if (palavras.includes(palavraFormada)) {
      console.log(`A palavra ${palavraFormada} está na lista.`);
      contador -= 1;
      resetPalavra();

      console.log("Palavras restantes:", contador);
      if (contador === 0) {
        exibirMensagemParabens();
      }
    }

    console.log("Palavras restantes:", contador);
  }

  function obterPalavraFormada() {
    return celulasSelecionadas
      .sort((a, b) => a.linha * 20 + a.coluna - b.linha * 20 + b.coluna)
      .map(({ linha, coluna }) => letras[linha][coluna])
      .join("");
  }
</script>

<div class="container">
  <div>
    <button class="btn-voltar">
      <a href="/">Menu</a>
    </button>
    <div class="tabela">
      <table>
        {#each letras as linha, i}
          <tr>
            {#each linha as letra, j}
              <td
                class="cell cell-{i}-{j}"
                on:mousedown={(event) => selecionarCelula(event, i, j)}
                on:mouseenter={(event) => selecionarCelula(event, i, j)}
              >
                {letra}
              </td>
            {/each}
          </tr>
        {/each}
      </table>
      <div class="li">
        <ul>
          {#each palavras as palavra}
            <li>{palavra}</li>
          {/each}
        </ul>
      </div>
    </div>
    <div class="contador">
      <div>
        <h2 class="elemento">Palavras restantes: {contador}</h2>
      </div>
      <div>
        <h3 class="elemento">{textoSelecionado}</h3>
      </div>

      <button class="elemento-btn" on:click={apagarConteudoSelecionado}
        >Apagar Selecionado</button
      >
    </div>
  </div>
</div>

<style>
  .selecionada {
    background-color: rgba(0, 255, 17, 0.792);
  }
</style>
