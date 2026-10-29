# Analisador Histórico de Loterias

Aplicação web executada diretamente no navegador para analisar resultados históricos de loterias, calcular frequência e atraso das dezenas e gerar combinações ponderadas.

> **Aviso importante:** este projeto é recreativo. Em sorteios independentes, resultados anteriores não tornam uma dezena mais provável no concurso seguinte. As combinações geradas não são previsões nem garantem prêmio. Aposte com responsabilidade e estabeleça um limite de gasto.

## Funcionalidades

- Atualização de resultados pela API pública da CAIXA.
- Importação manual de arquivos CSV, XLSX, XLS, HTML e HTM.
- Reconhecimento de cabeçalhos comuns, como `Data do Sorteio`, `Bola 1`, `1ª Dezena`, `Dezena 1` e `Número 1`.
- Filtros por últimos 365 dias, mês e ano, ano completo, intervalo personalizado ou todo o histórico disponível.
- Cálculo de frequência de cada dezena.
- Cálculo do atraso em quantidade de concursos.
- Ajuste do peso de frequência e atraso.
- Geração de 1 a 100 combinações ponderadas.
- Ranking histórico das dezenas.
- Exportação das combinações e do ranking para CSV.
- Interface responsiva com suporte aos temas claro e escuro.

## Loterias disponíveis

- Mega-Sena
- Lotofácil
- Quina
- Lotomania
- Timemania, considerando somente as dezenas
- Dupla Sena, considerando cada sorteio separadamente
- Dia de Sorte, considerando somente as dezenas
- +Milionária, considerando somente as dezenas

Recursos complementares, como Time do Coração, Mês da Sorte e trevos da +Milionária, não fazem parte da análise atual.

## Como executar

Não é necessário instalar a aplicação.

1. Baixe ou copie o arquivo `index.html`.
2. Abra o arquivo no Microsoft Edge, Google Chrome ou Firefox.
3. Mantenha a conexão com a internet ativa para usar a atualização automática e importar planilhas Excel.

## Atualização automática

1. Escolha a modalidade no campo **Loteria**.
2. Clique em **Atualizar pela CAIXA**.
3. Aguarde a mensagem indicando quantos concursos foram carregados.
4. Selecione o período da análise.
5. Clique em **Analisar e gerar apostas**.

A atualização automática consulta o concurso mais recente e percorre os concursos anteriores até reunir aproximadamente os últimos 370 dias. O filtro escolhido é aplicado depois do carregamento.

Se o navegador bloquear a requisição por política de segurança, utilize a importação manual descrita abaixo.

## Importação manual

O programa aceita estes formatos:

- `.csv`
- `.xlsx`
- `.xls`
- `.html`
- `.htm`

### CSV simples

Exemplo para a Mega-Sena:

```csv
data;bola1;bola2;bola3;bola4;bola5;bola6
01/01/2026;4;13;23;28;41;58
08/01/2026;1;9;18;30;44;55
```

Também são reconhecidos nomes como:

- `Data do Sorteio`
- `Data Concurso`
- `Bola 1`, `Bola 2` etc.
- `Dezena 1`, `Dezena 2` etc.
- `1ª Dezena`, `2ª Dezena` etc.
- `Número 1`, `Número 2` etc.
- Uma única coluna chamada `Dezenas`, `Dezenas sorteadas`, `Números sorteados` ou `Resultado`

Para importar:

1. Escolha a loteria correspondente ao arquivo.
2. Clique no seletor **Histórico da CAIXA**.
3. Selecione o arquivo.
4. Defina o período.
5. Clique em **Analisar e gerar apostas**.

## Critério estatístico

A aplicação combina dois componentes:

- **Frequência:** quantidade de vezes que cada dezena apareceu no período selecionado.
- **Atraso:** quantidade de concursos decorridos desde a última aparição da dezena.

O valor padrão é:

- 80% para frequência.
- 20% para atraso.

Ao alterar a frequência, o peso do atraso é ajustado automaticamente para que a soma permaneça em 100%.

A frequência recebe suavização de Laplace, adicionando uma unidade à contagem de cada número. Isso evita peso zero para dezenas que não apareceram no período.

As combinações são sorteadas sem repetição, usando os pesos calculados. Portanto, executar a análise novamente pode produzir combinações diferentes com os mesmos dados.

## Filtros de período

- **Últimos 365 dias do arquivo:** usa como referência a data mais recente disponível nos dados.
- **Mês e ano:** utiliza o mês da data informada no campo inicial.
- **Ano:** utiliza o ano da data informada no campo inicial.
- **Datas personalizadas:** exige uma data inicial e uma data final.
- **Todo o histórico:** utiliza todos os concursos carregados.

## Exportação

Depois da análise, clique em **Exportar resultado CSV**. O arquivo `analise_loteria.csv` contém:

1. As combinações sugeridas.
2. A frequência de cada dezena.
3. O atraso em concursos.
4. O peso percentual calculado pelo modelo.

## Dependências externas

O arquivo usa a biblioteca SheetJS para ler arquivos Excel:

```text
https://cdn.jsdelivr.net/npm/xlsx@0.18.5/dist/xlsx.full.min.js
```

Também consulta resultados na API pública do Portal de Loterias da CAIXA:

```text
https://servicebus2.caixa.gov.br/portaldeloterias/api/
```

Por isso, as funções de atualização automática e leitura de Excel podem depender de conexão com a internet. Arquivos CSV e HTML podem ser processados localmente.

## Privacidade

Os arquivos selecionados são processados no navegador. O código não envia o arquivo importado para um servidor próprio. Na atualização automática, o navegador faz solicitações à API da CAIXA. Para carregar o leitor de Excel, o navegador acessa o CDN da biblioteca SheetJS.

## Limitações conhecidas

- A atualização automática depende da disponibilidade e da política de acesso da API da CAIXA.
- Navegadores podem bloquear requisições feitas a partir de um arquivo local.
- O leitor de Excel depende do carregamento da biblioteca externa.
- Trevos, times e meses especiais não são analisados.
- A Federal, a Loteca e modalidades com regras não baseadas apenas em conjuntos de dezenas não estão incluídas.
- O resultado é uma ponderação histórica, não uma probabilidade oficial do próximo sorteio.

## Estrutura do projeto

```text
index.html   Aplicação completa: interface, estilos e JavaScript
README.md    Documentação do projeto
```

## Solução de problemas

### “Não foi possível atualizar automaticamente”

- Confirme que o dispositivo está conectado à internet.
- Tente abrir o arquivo em outro navegador.
- Caso o bloqueio persista, baixe o histórico e utilize a importação manual.

### “Não encontrei a coluna de data”

Verifique se o arquivo possui uma coluna como `Data`, `Data do Sorteio` ou `Data Concurso`.

### “Há números fora da faixa da loteria selecionada”

Confirme se a modalidade selecionada corresponde ao histórico importado.

### O arquivo Excel não abre

Confirme a conexão com a internet, pois a biblioteca usada para interpretar XLS e XLSX é carregada externamente.

## Uso responsável

Este software não deve ser apresentado como sistema de previsão, garantia de prêmio ou método para superar as probabilidades oficiais. Use-o apenas para estudo, recreação e exploração de históricos.

## Licença

Nenhuma licença foi definida neste repositório. Antes de distribuir, modificar ou publicar o projeto, adicione uma licença compatível com o uso pretendido.
