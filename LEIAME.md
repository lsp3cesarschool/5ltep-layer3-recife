# 5LTEP-L3 · instância Recife (experimento de controle)

[![Tests](https://github.com/lsp3cesarschool/5ltep-layer3-recife/actions/workflows/tests.yml/badge.svg)](https://github.com/lsp3cesarschool/5ltep-layer3-recife/actions/workflows/tests.yml) [![Camada 3 · trânsito](https://img.shields.io/endpoint?url=https%3A%2F%2Fraw.githubusercontent.com%2Flsp3cesarschool%2F5ltep-layer3-recife%2Fmain%2Fdocs%2Fdata%2Fstatus-recife-infracoes-transito.pt.json)](https://github.com/lsp3cesarschool/5ltep-layer3-recife/actions/workflows/layer3.yml) [![Camada 3 · meio ambiente](https://img.shields.io/endpoint?url=https%3A%2F%2Fraw.githubusercontent.com%2Flsp3cesarschool%2F5ltep-layer3-recife%2Fmain%2Fdocs%2Fdata%2Fstatus-recife-autos-infracao-ambiental.pt.json)](https://github.com/lsp3cesarschool/5ltep-layer3-recife/actions/workflows/layer3.yml) [![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

[English](README.md) · **Português**

**Detecção de anomalias da Camada 3 do 5L-TEP aplicada a dois conjuntos do portal de dados abertos da
Prefeitura do Recife: uma terceira instância do [5ltep-layer3](https://github.com/lsp3cesarschool/5ltep-layer3/blob/main/LEIAME.md),
montada pelo autor como caso de controle do estudo do IBAMA, ao lado da
[instância ANEEL](https://github.com/lsp3cesarschool/5ltep-layer3-aneel/blob/main/LEIAME.md).**

| Recurso | O que tem lá |
|---|---|
| 📊 **Painel** | [lsp3cesarschool.github.io/5ltep-layer3-recife](https://lsp3cesarschool.github.io/5ltep-layer3-recife/?lang=pt): anomalias, rótulos do LLM, decisões do gestor, verificações da fonte e a proveniência de cada resultado (um seletor *Dataset* alterna entre os dois perfis) |
| 🧑‍⚖️ **Fila de revisão** | [![revisões abertas](https://img.shields.io/github/issues/lsp3cesarschool/5ltep-layer3-recife/layer3?label=revis%C3%B5es%20abertas&color=0366d6)](https://github.com/lsp3cesarschool/5ltep-layer3-recife/issues?q=is%3Aissue+is%3Aopen+label%3Alayer3) [![pendentes](https://img.shields.io/github/issues/lsp3cesarschool/5ltep-layer3-recife/review%3Apending?label=pendentes&color=d73a4a)](https://github.com/lsp3cesarschool/5ltep-layer3-recife/issues?q=is%3Aissue+is%3Aopen+label%3Areview%3Apending)<br>contagens ao vivo; cada selo abre sua lista de issues |
| 🏛️ **Instância principal** | [5ltep-layer3](https://github.com/lsp3cesarschool/5ltep-layer3/blob/main/LEIAME.md): IBAMA, e a documentação completa |
| 🧪 **Escolha do modelo** | [5ltep-layer3-modeltest](https://github.com/lsp3cesarschool/5ltep-layer3-modeltest/blob/main/LEIAME.md): o benchmark mensal que escolhe o LLM juiz |
| 🔒 **Segurança** | [SECURITY.md](SECURITY.md): o que não é confiável (o modelo, o portal de dados, fontes da web), como o kit o contém, e como relatar uma vulnerabilidade |

> **Situação: demonstração de pesquisa.** Este repositório não é operado pela Prefeitura do Recife,
> pela CTTU nem pela Secretaria de Meio Ambiente e Sustentabilidade, não tem vínculo com elas nem aval
> delas; apenas lê os dados abertos do município. Ele mostra que o kit pode ser reutilizado em outro
> portal. Não pressupõe que a prefeitura vá revisar seus resultados ou adotá-lo. As issues de revisão
> abertas demonstram o fluxo: não há gestor designado.

## Caso de uso em um parágrafo

A Prefeitura do Recife publica, no seu portal CKAN [dados.recife.pe.gov.br](https://dados.recife.pe.gov.br),
cada infração de trânsito registrada pela CTTU e os autos de infração ambiental lavrados pela Secretaria
de Meio Ambiente e Sustentabilidade. Suponha que a prefeitura, ou qualquer pessoa que reutilize esses
dados, queira saber se o registro publicado é confiável e onde olhar primeiro se algo estiver errado.
Esta camada monta o número mensal de registros de cada conjunto, encontra os meses que fogem do padrão
habitual e usa um modelo de IA local para verificar quais desvios têm explicação conhecida (uma nova lei
de trânsito, o Carnaval, uma troca de gestão) e quais parecem um **problema nos dados** (um mês sem
registros, um lote lançado de uma vez). Os não explicados vão primeiro para uma pessoa; a IA só propõe,
e um gestor de dados confirma ou corrige toda decisão que leve a uma ação.

## Por que um experimento de controle

Um método que funciona num conjunto de dados pode ter aprendido só as manias desse conjunto. IBAMA
(federal, ambiental, um zip) e ANEEL (federal, setor elétrico, um CSV) são órgãos federais; o Recife é
um portal **municipal**, e seus dois conjuntos diferem dos dois na forma de publicação. Este repositório
roda **o mesmo código**, seguindo os passos que outra instituição seguiria para adotá-lo, e faz as
mesmas duas perguntas da instância ANEEL:

1. **Reutilização (o *R* do FAIR):** o kit pode ser adotado escrevendo um perfil, sem mudar código?
2. **Transferência:** detecção, LLM-as-a-Judge e protocolo de revisão se comportam do mesmo jeito numa
   série municipal muito grande (trânsito) e numa muito curta (meio ambiente)?

O que difere do repositório principal é só:

| Arquivo | Por quê |
|---|---|
| [`profiles/recife-infracoes-transito.json`](profiles/recife-infracoes-transito.json) | infrações de trânsito: portal, recursos anuais, colunas, formatos de data, séries, texto de domínio para o LLM |
| [`profiles/recife-autos-infracao-ambiental.json`](profiles/recife-autos-infracao-ambiental.json) | autos de infração ambiental, do mesmo jeito |
| [`profiles/events/`](profiles/events/) | um calendário de eventos por perfil, começado pequeno e feito para ser preenchido com *Suggest events* e conferido por um gestor |
| [`profiles/i18n/`](profiles/i18n/) | textos em português dos dois perfis para o painel |
| este LEIAME e sua versão em inglês, [`README.md`](README.md) | |

Os perfis e resultados do IBAMA não estão incluídos. Todo o resto (detectores, LLM-as-a-Judge, revisão
humana, verificações da fonte, painel, workflows, testes) é idêntico a
[`5ltep-layer3@65fa850`](https://github.com/lsp3cesarschool/5ltep-layer3/tree/65fa8503f94d6e41f5350c455f739453ba4bcf9a),
e a instância montada passa pela mesma suíte de testes antes de cada atualização.

## Termos-chave

| Termo | Significado aqui |
|---|---|
| **Anomalia** | um mês de uma série mensal (número de registros) que foge do padrão habitual, apontado por pelo menos 2 de 4 detectores estatísticos |
| **Mudança de nível** | uma mudança duradoura de patamar (não um pico de um mês), detectada por um teste de Page-Hinkley |
| **LLM-as-a-Judge** | um modelo de linguagem pequeno, rodado localmente e sem custo, que lê cada anomalia com seu contexto e diz qual das quatro causas abaixo a explica melhor |
| **Verificação da fonte** | um problema dos próprios arquivos publicados (formatos de data, bytes inválidos, atraso de publicação), mostrado no painel como tratado ou como alerta |
| **Gestor de dados** | a pessoa que confirma ou corrige o rótulo do juiz (por uma issue do GitHub) antes de qualquer ação |

As quatro causas (categorias) entre as quais o juiz escolhe:

| Código | Categoria | Exemplo (Recife) | O que significa |
|---|---|---|---|
| **PDC** | Mudança por política | as infrações mudam depois de uma alteração do Código de Trânsito ou de novos radares | explicada por um evento conhecido: documentar |
| **SP** | Padrão sazonal | menos registros todo Carnaval ou fim de ano | comportamento esperado: nenhuma ação |
| **DQE** | Evento de qualidade de dados | um mês sem registros numa série ativa; meses lançados em lote | um **problema nos dados**: sempre revisado por um gestor |
| **GES** | Mudança real de fiscalização | uma mudança duradoura na intensidade da fiscalização, sem evento e sem sinais nos dados | uma mudança real que ninguém explicou ainda |

## Como os dados do Recife diferem dos do IBAMA

| | IBAMA (estudo principal) | Recife: trânsito (CTTU) | Recife: meio ambiente |
|---|---|---|---|
| Registros | ~711 mil autos desde 1977 | ~8,5 milhões de infrações, 2006-2025 | ~3.200 autos, 2021-2023 |
| Recurso | um zip com um CSV por ano | **um recurso por ano** (20 CSVs, ~2,4 GB) | um recurso por ano (3 CSVs) |
| Coluna de data | `DAT_HORA_AUTO_INFRACAO` | `datainfracao`, em **três formatos de data** entre os anos, um deles com o dia primeiro (`02/01/2024`) | `DATA DA VISTORIA` |
| Colunas de chave / valor | `SEQ_AUTO_INFRACAO` / `VAL_AUTO_INFRACAO` | nenhuma / nenhuma (só contagem) | `Nº AUTO` / nenhuma (só contagem) |
| Último mês publicado | o corrente | **meses de atraso** (arquivos anuais) | **2023-12** |
| Volume mensal | centenas a milhares | dezenas de milhares | cerca de 100 |
| Calendário de eventos | curado, ~14 eventos verificados | 9 eventos verificados, a ampliar | 2 eventos verificados, a ampliar |
| Licença dos dados | dados abertos do IBAMA | ODbL | ODbL |

## Como funciona

O pipeline é o documentado no [LEIAME principal](https://github.com/lsp3cesarschool/5ltep-layer3/blob/main/LEIAME.md);
em resumo:

```
┌────────────────────────────────────────────────────────────┐
│  GitHub Actions: cron mensal + botão "Run workflow"        │
│  (runner de repositório público: 4 vCPU / 16 GB, gratuito) │
└─────────────────────────────┬──────────────────────────────┘
                              ▼
 ① Fonte      API CKAN de dados.recife.pe.gov.br → cada CSV anual (SHA-256 de cada um registrado)
 ② Verificar  verificações da fonte: formatos de data, bytes inválidos, atraso de publicação (no painel)
 ③ Agregar    só as colunas de data e identificador (nenhum nome, endereço ou CPF/CNPJ guardado)
 ④ Detectar   Z-score · MAD · Isolation Forest · LSTM-ED (voto ≥ 2) + Page-Hinkley
 ⑤ Julgar     Ollama + o modelo aprovado pelo benchmark, 3 execuções com semente, resposta JSON
 ⑥ Revisar    issues do GitHub: rótulo steward:<CATEGORIA> + fechar (review:pending para DQE)
 ⑦ Relatório  layer3_summary.json (l3_rate, l3_pass, anomaly_flags) + painel
                              ▼
       Histórico Git de cada série, detecção, julgamento e revisão
```

- **Um arquivo por ano:** os perfis indicam os recursos por um padrão (`resource_pattern`), então o
  arquivo de um ano novo entra sozinho; cada arquivo é lido e apagado antes de o próximo ser baixado.
- **A série termina no último mês publicado** (`period.end: "last_record"`): os meses seguintes ainda
  não foram publicados, e contá-los como zero pareceria um colapso.
- **Lotes:** cada execução julga até 25 anomalias, faz o commit e inicia o próximo lote até não sobrar
  nada pendente.
- **Sem rejulgamento por padrão:** cada julgamento guarda uma impressão digital dos seus próprios dados
  e o modelo e a versão do prompt que o produziram. Só é julgado de novo se seus dados mudarem, ou se um
  gestor pedir.
- **Modelo:** `LLM_MODEL=auto` (padrão) usa o modelo que o
  [benchmark](https://github.com/lsp3cesarschool/5ltep-layer3-modeltest/blob/main/LEIAME.md) aprova no
  momento.
- **Calendário de eventos:** *Suggest events* pede ao LLM eventos ancorados nas páginas da Wikipédia
  "*ano* no Brasil" e abre um pull request; as sugestões ficam marcadas `suggested` até um gestor
  verificá-las.

## O que o experimento de controle mostrou

*Os resultados de detecção e julgamento aparecem no painel depois da primeira execução no GitHub
Actions; serão resumidos aqui a partir dela, não de ensaios locais.*

O que a montagem da instância já mostrou:

- **A reutilização pediu três campos novos de perfil, no código comum.** O portal do Recife publica os
  dois conjuntos em um recurso por ano, com nomes que nem são consistentes ("Registro **das**
  infrações", "Registro **de** Infrações", "Registro **da** infrações"). Daí vieram `resource_pattern`
  (uma expressão regular para os recursos anuais), `date_formats` (os arquivos de trânsito usam
  `AAAA-MM-DD`, `DD/MM/AAAA hh:mm` e `AAAA/MM/DD`, com e sem hora) e `period.end: "last_record"`. Um
  quarto, `encoding_errors`, foi acrescentado quando uma amostra do arquivo de 2014 pareceu inválida em
  UTF-8; a verificação de codificação mostrou depois que o arquivo inteiro é válido (a amostra tinha
  cortado um caractere ao meio), então esta instância não o usa. Todos foram para o repositório
  principal, e IBAMA e ANEEL rodam o mesmo código.
- **Problemas nos arquivos agora são informados, não só contornados.** As
  [verificações da fonte](https://github.com/lsp3cesarschool/5ltep-layer3/blob/main/LEIAME.md#verificações-da-fonte)
  foram escritas para esta instância e, na primeira execução, encontraram uma quarta forma de data num
  arquivo anual (2018) que o perfil não previa. Sem elas, um arquivo com o dia primeiro lido com o mês
  primeiro levaria registros de um mês para outro, e os meses que faltam depois do último arquivo anual
  pareceriam um colapso, os dois em silêncio.
- **A série ambiental fica no mínimo:** 36 meses, o menor número que os detectores aceitam. Ela testa
  até onde o kit pode ir, não como ele se sai numa série confortável.

## Como rodar, e adaptar de novo

| Workflow | Quando | O quê |
|---|---|---|
| `layer3.yml` | dia 5 de cada mês, e manual (botões *Run Layer 3 now* e *Re-judge* do painel) | detectar → julgar em lotes → issues de revisão → relatório, para os dois perfis |
| `reviews.yml` | quando uma issue `layer3` recebe rótulo ou é fechada | registra as decisões do gestor, atualiza o painel |
| `events.yml` | manual (botão *Suggest events* do painel) | sugestões de eventos do LLM → pull request |
| `model-check.yml` | dia 22 de cada mês | só se o modelo estiver fixado: propõe a escolha do benchmark |
| `tests.yml` | push / pull request | suíte de testes |

Esta instância é ela própria uma receita para outra: faça um fork do repositório principal, substitua os
perfis pelos seus (`python main.py check-profile --profile profiles/<seu>.json` valida um perfil contra
o portal real e roda as verificações da fonte), marque-o `"scheduled": true`, apague `data/`,
`results/` e `docs/data/`, ative Actions e Pages e rode o workflow uma vez. O LEIAME principal detalha
cada passo e cada campo do perfil.

## Reprodutibilidade

Cada execução registra o SHA-256 de cada arquivo analisado (e um sobre todos eles) e do perfil, as
verificações da fonte, cada parâmetro do método, as versões do Python e dos pacotes e, para cada
julgamento, o prompt completo, as três respostas com suas sementes, o digest do modelo e a versão do
prompt. As séries mensais são versionadas, então a detecção pode ser refeita exatamente sobre os dados
de qualquer commit passado com `python main.py detect --from-series`, mesmo que os arquivos do portal
mudem com o tempo.

## Limitações

- A série de trânsito conta infrações, não autos nem multas: os arquivos não têm número do auto nem
  valor, então duplicatas não podem ser detectadas e nenhuma série de valor pode ser montada.
- A série ambiental é curta (36 meses) e para em 2023: a sazonalidade e o LSTM-ED têm pouco de onde
  aprender.
- Os dois conjuntos são publicados com meses ou anos de atraso, então a Camada 3 não vê os meses
  recentes; o próprio atraso é informado como verificação da fonte.
- Os calendários de eventos são pequenos: a maioria das anomalias é julgada sem o contexto que um
  especialista local traria.
- Nenhuma decisão de gestor, por desenho: o fluxo de revisão funciona e está pronto para adoção, e as
  métricas de revisão se preenchem sozinhas se e quando um gestor o usar. O autor não faz o papel de
  gestor, o que seria autoavaliação.
- Valem os [limites de tamanho e de tempo](https://github.com/lsp3cesarschool/5ltep-layer3/blob/main/LEIAME.md#limites-de-tamanho-e-de-tempo)
  da instância principal.

## Documentação e referências

Método, parâmetros, protocolo de revisão humana, notas sobre FAIR e replicabilidade, e como adaptar o
kit: **<https://github.com/lsp3cesarschool/5ltep-layer3/blob/main/LEIAME.md>**. Escolha do modelo:
**<https://github.com/lsp3cesarschool/5ltep-layer3-modeltest/blob/main/LEIAME.md>**.

- Pinheiro, L. S., et al. (2026). *Towards Trust Engineering in Open Data Systems: A Layered Conceptual Framework Integrating Quality Assurance and Governance Perspectives*. SOFTENG 2026, IARIA, pp. 21–28.
- Prefeitura do Recife. *Registro das Infrações de Trânsito*. Dados Recife. <https://dados.recife.pe.gov.br/dataset/registro-das-infracoes-de-transito>
- Prefeitura do Recife. *Autos de infração ambiental*. Dados Recife. <https://dados.recife.pe.gov.br/dataset/autos-de-infracao-ambiental>

## Licença

Código: MIT, ver [LICENSE](LICENSE). As séries mensais em `data/` são agregados derivados dos dados
abertos da Prefeitura do Recife (ODbL); ao reutilizá-las, cite o portal Dados Recife como fonte
original.
