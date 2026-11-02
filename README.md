# Acidentes nas rodovias federais do Sudeste em 2025

Projeto desenvolvido para a disciplina de Análise Exploratória de Dados e Visualização da FGV EMAp.

Analisamos **23.323 acidentes registrados pela Polícia Rodoviária Federal (PRF)** em Minas Gerais, Espírito Santo, Rio de Janeiro e São Paulo durante 2025. O objetivo é entender a distribuição das ocorrências e comparar a proporção de acidentes graves em diferentes situações.

Consideramos grave o acidente com pelo menos uma pessoa morta ou gravemente ferida. Cada ocorrência é contada uma única vez, mesmo quando apresenta as duas situações.

## Perguntas analisadas

1. Como os acidentes registrados se distribuem entre os estados e as rodovias do Sudeste?
2. Quais meses, dias da semana e faixas horárias concentram mais ocorrências?
3. Quais tipos de acidente apresentam maior proporção de ocorrências graves?
4. A proporção de acidentes graves difere entre dia e noite, em pistas simples e duplas?
5. Como essa proporção varia entre condições meteorológicas, considerando cada estado?
6. As diferenças de gravidade entre condições meteorológicas permanecem em comparações na mesma rodovia e em faixas horárias semelhantes?
7. As rodovias com mais acidentes registrados também apresentam maior proporção de ocorrências graves?

## Organização do projeto

```text
README.md
src/
├── __main__.ipynb
└── datatran2025.csv
```

O notebook reúne a preparação dos dados, os cálculos, as visualizações e as conclusões.

O arquivo `datatran2025.csv` contém a base nacional utilizada. A seleção dos quatro estados do Sudeste é feita no próprio notebook.

## Fontes dos dados

- [PRF — Dados abertos](https://www.gov.br/prf/pt-br/acesso-a-informacao/dados-abertos/dados-abertos-da-prf): arquivo de acidentes de 2025 agrupados por ocorrência. A versão utilizada contém 72.529 registros nacionais e 30 colunas.
- [Infrasa — Rodovias federais](https://geo.infrasa.gov.br/server/rest/services/Hosted/Rodovias_Federais/FeatureServer/0): malha utilizada como referência espacial. A versão identificada na análise foi a SNV 202601A.
- [IBGE — Serviço de malhas territoriais](https://servicodados.ibge.gov.br/api/v3/malhas/regioes/SE?formato=application%2Fvnd.geo%2Bjson&intrarregiao=UF&qualidade=minima): limites estaduais utilizados no mapa.
- [OpenStreetMap](https://www.openstreetmap.org/copyright): fundo dos mapas.

A malha da Infrasa possui referência temporal diferente dos acidentes de 2025. Essa diferença foi documentada como uma limitação da análise.

## Como consultar o trabalho

O arquivo principal é `src/__main__.ipynb`.

As visualizações foram salvas como imagens dentro do notebook para facilitar sua leitura no GitHub. Para consultar os resultados salvos, não é necessário executar o código.

Para reproduzir os cálculos e gerar novamente as figuras, siga as instruções abaixo.

## Como executar

O ambiente utilizado no desenvolvimento foi **Python 3.14.6**, com [JupyterLab](https://jupyterlab.readthedocs.io/en/stable/getting_started/installation.html).

Baixe o repositório e abra um terminal na pasta principal do projeto. No Windows, usando PowerShell, execute:

```powershell
python -m venv .venv
```

Instale as principais bibliotecas nas versões utilizadas no projeto:

```powershell
.\.venv\Scripts\python.exe -m pip install jupyterlab==4.5.8 ipython==9.14.1 pandas==3.0.3 geopandas==1.2.0 shapely==2.2.0

.\.venv\Scripts\python.exe -m pip install altair==6.3.0 matplotlib==3.11.0 contextily==1.7.1

.\.venv\Scripts\python.exe -m pip install requests==2.34.2 truststore==0.10.5 vl-convert-python==1.9.0.post1
```

Abra o JupyterLab a partir da pasta `src`:

```powershell
cd src
..\.venv\Scripts\python.exe -m jupyterlab
```

No JupyterLab, abra `__main__.ipynb` e execute as células na ordem em que aparecem.

É importante iniciar dentro de `src`, pois o notebook procura o CSV nessa pasta.

Os comandos utilizam diretamente o Python do ambiente virtual, sem exigir sua ativação no PowerShell. O pacote `vl-convert-python` é utilizado para gerar as imagens PNG dos gráficos feitos com Altair.

A execução completa precisa de internet para consultar a malha da Infrasa, os limites do IBGE e o fundo dos mapas. A indisponibilidade desses serviços pode interromper as etapas geográficas. Como a camada da Infrasa pode ser atualizada, uma nova execução também pode carregar uma versão diferente da documentada.

## Preparação dos dados

A preparação inclui verificações de:

- Valores ausentes e campos de texto.
- IDs repetidos e linhas duplicadas.
- Datas, horários e compatibilidade com o dia da semana.
- Categorias desconhecidas ou não identificadas.
- Contagens negativas, fracionárias ou não numéricas.
- Consistência entre totais e categorias de pessoas.
- Coordenadas fora das faixas globais válidas e quilometragens negativas.

Os registros com informações desconhecidas foram tratados conforme a variável necessária em cada pergunta.

Foram preservadas 886 ocorrências com divergências entre o total de pessoas e a soma das categorias de situação física. Essa diferença foi documentada, pois não havia fundamento suficiente para determinar uma correção.

## Análise espacial

Selecionamos os trechos de jurisdição federal dos quatro estados e excluímos os classificados como planejados.

A malha foi organizada por UF e BR e dividida em segmentos de até 10 km. Para cada acidente, procuramos a linha mais próxima com a mesma UF e BR, respeitando o limite de 200 metros e os demais critérios descritos no notebook.

Foram associados **22.008 acidentes**, correspondentes a **94,36%** do recorte. Os registros não associados continuam nas análises que não dependem dessa associação espacial.

As cores do mapa representam acidentes associados por quilômetro, calculados em segmentos de até 10 km. O segmento final de cada linha pode ser menor, e sua extensão efetiva é utilizada no cálculo.

Os trechos cinza não tiveram registros associados pelos critérios utilizados. Isso não comprova ausência de acidentes nesses locais.

## Principais resultados

- Minas Gerais reuniu 41,03% dos registros do Sudeste.
- A tarde apresentou o maior total entre as quatro faixas horárias, com 31,44% das ocorrências.
- Atropelamentos de pedestres e colisões frontais se destacaram pelas proporções de acidentes graves.
- Nas pistas simples e duplas, a proporção de graves foi maior à noite.
- As rodovias com mais registros não apresentaram necessariamente as maiores proporções de acidentes graves.

As interpretações completas e as contagens de cada comparação estão no notebook.

## Limitações

Os dados representam acidentes registrados pela PRF e não abrangem todos os acidentes ocorridos no Sudeste.

Sem informações sobre o fluxo de veículos, não estimamos risco por viagem. As proporções indicam a parcela de ocorrências graves entre os acidentes registrados em cada grupo.

Grupos com poucos registros podem apresentar percentuais extremos. Por isso, os gráficos mostram as contagens que sustentam as proporções.

A associação espacial utiliza critérios exploratórios, e a malha SNV 202601A pode apresentar diferenças em relação à rede existente em 2025.

Os resultados descrevem padrões e associações nos registros, sem estabelecer relações de causa e efeito.

## Autores

- Miguel Vitor Pereira
- Ricardo Rodrigues Tose
- Daniel Furtado Nunes Rocha da Silva

**Professor:** João Lucas Rulff da Costa  
**Instituição:** FGV EMAp  
**Disciplina:** Análise Exploratória de Dados e Visualização  
**Período:** 2026.2