# 5LTEP-L4 · instância Recife (experimento de controle)

[![Tests](https://github.com/lsp3cesarschool/5ltep-layer4-recife/actions/workflows/tests.yml/badge.svg)](https://github.com/lsp3cesarschool/5ltep-layer4-recife/actions/workflows/tests.yml) [![Layer 4](https://img.shields.io/endpoint?url=https%3A%2F%2Fraw.githubusercontent.com%2Flsp3cesarschool%2F5ltep-layer4-recife%2Fmain%2Fdocs%2Fdata%2Fstatus.pt.json)](https://github.com/lsp3cesarschool/5ltep-layer4-recife/actions/workflows/monitor.yml) [![Cross-check](https://img.shields.io/endpoint?url=https%3A%2F%2Fraw.githubusercontent.com%2Flsp3cesarschool%2F5ltep-layer4-recife%2Fmain%2Fdocs%2Fdata%2Fstatus-cross-check.pt.json)](https://github.com/lsp3cesarschool/5ltep-layer4-recife/actions/workflows/cross_check.yml) [![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

[English](README.md) · **Português**

**Camada 4 do 5L-TEP (observabilidade e proveniência) aplicada ao portal de dados abertos da Prefeitura do Recife:
outra instância do [5ltep-layer4](https://github.com/lsp3cesarschool/5ltep-layer4), montada pelo autor como caso de controle do
estudo do IBAMA.**

| Recurso | O que você encontra lá |
|---|---|
| 📊 **Painel** | [lsp3cesarschool.github.io/5ltep-layer4-recife](https://lsp3cesarschool.github.io/5ltep-layer4-recife/?lang=pt): mudanças por mês, onde estão os arquivos, últimas mudanças, saúde do monitoramento, todos os conjuntos |
| 📄 **Registro de mudanças** | [`changes.md`](changes.md) (em inglês): cada mudança detectada em palavras simples, reconstruído a cada ciclo |
| 🔗 **Registros de proveniência** | [`provenance_logs/`](provenance_logs/): uma cadeia W3C PROV-DM (JSON-LD) somente por acréscimo, por conjunto |
| 🏛️ **Instância principal** | [5ltep-layer4](https://github.com/lsp3cesarschool/5ltep-layer4): IBAMA, e a documentação completa |
| 🔁 **Outro controle** | [5ltep-layer4-aneel](https://github.com/lsp3cesarschool/5ltep-layer4-aneel): o portal da ANEEL |

> **Situação: demonstração de pesquisa.** Este repositório não é operado, afiliado ou endossado pela Prefeitura do Recife nem pela EMPREL; ele apenas lê os dados abertos da cidade. Ele mostra que o kit pode ser
> reutilizado em outro portal e não pressupõe que o publicador vá revisar seus resultados ou adotá-lo.
> Os alertas chegam ao mantenedor deste repositório, não ao publicador.

## Caso de uso em um parágrafo

A Prefeitura do Recife publica seus dados abertos num portal CKAN: centenas de conjuntos sobre
saúde, educação, transporte, orçamento e serviços urbanos, muitos atualizados por ano ou por mês.
Suponha um jornalista ou um pesquisador que reutiliza um deles ao longo dos meses: se um arquivo
anual for substituído no mesmo endereço, um recurso for removido ou um conjunto for editado sem
nova data de modificação, a análise publicada deixa de corresponder ao portal e ninguém sabe dizer
quando divergiu. Esta instância lê os metadados de todos os conjuntos do Recife a cada seis horas e
registra cada mudança como uma entidade W3C PROV-DM ligada à versão anterior, com o órgão da
prefeitura como custodiante, para que qualquer pessoa veja quando um conjunto mudou, como, e se a
mudança foi datada.

## Por que um experimento de controle

O kit da Camada 4 foi construído e avaliado no portal do IBAMA. O Recife é um município, com outra equipe e outros hábitos de publicação
(cerca de 220 conjuntos, o maior dos três portais; um ciclo leva cerca de um minuto). Este repositório roda **o mesmo código**, seguindo os passos de *Monitorando outro
portal CKAN* do README principal: só o `portal.json` (a URL do portal) e os textos deste README
mudaram. Onde o Recife publica de outro jeito que o IBAMA, a diferença aparece nos registros de
mudança, não no código. O monitoramento começa com uma linha de base: o primeiro ciclo não registra
mudança, só o estado com que todos os ciclos seguintes são comparados.

## O que mudou em relação à instância principal

| Arquivo | Mudança |
|---|---|
| `portal.json` | `portal_url` = `https://dados.recife.pe.gov.br` |
| `README.md`, `LEIAME.md`, `CITATION.cff` | este texto e a citação deste repositório |

Todo o resto é o código do `5ltep-layer4` no commit `2cd236e`
([2cd236e38bd07b4813d3897bb6874d0e6d33875d](https://github.com/lsp3cesarschool/5ltep-layer4/commit/2cd236e38bd07b4813d3897bb6874d0e6d33875d)). Os snapshots, os registros de
proveniência, o registro de mudanças e os dados do painel são gravados aqui pelos workflows.

## Como rodar, e como adaptar de novo

O workflow *5L-TEP Layer 4 Monitoring Workflow* roda a cada seis horas e pode ser iniciado à mão
(*Actions → Run workflow*); a verificação cruzada roda uma vez por dia. Localmente:

```bash
pip install -r requirements.txt
python main.py --max-datasets 5 --dry-run
```

Para apontar para outro portal CKAN, mude `portal_url` no `portal.json`; veja o README principal.

## Reprodutibilidade

Cada registro de proveniência traz a impressão digital da versão do conjunto que descreve, a
execução que a observou e o commit do código que rodou (`5ltep:commitSha`). O código é o commit de
origem acima; todo o resto é gravado pelos workflows, de modo que o histórico deste repositório é o
histórico do portal tal como observado.

## Limitações

As limitações da instância principal valem aqui. Específico do Recife: visto dos runners do GitHub, o portal recusa conexões novas de vez em quando. O coletor mantém uma conexão aberta e desiste de uma recusada em segundos, mas um ciclo ainda pode perder um conjunto; a perda é registrada como erro de coleta, nunca como mudança, e o ciclo seguinte lê o conjunto de novo.

## Documentação e referências

A documentação completa (taxonomia de mudanças, mapeamento PROV-DM, modelo de dois agentes,
verificação cruzada, configuração, avaliação e referências) está no
[repositório principal](https://github.com/lsp3cesarschool/5ltep-layer4/blob/main/LEIAME.md).

## Licença

MIT para o código ([LICENSE](LICENSE)). Os dados da prefeitura são publicados sob a Open Database License (ODbL); este repositório guarda só snapshots de metadados e registros de proveniência derivados deles.
