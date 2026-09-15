# Sprint 01

*Período:* 01/09/2026 a 14/09/2026

## Entregas da Sprint

| Trio de Desenvolvedores | Qtd. de US | US | Assunto das US |
|---|---:|---|---|
| Davi Emanuel / João Pedro Ferreira / Renan Vieira | 3 | US01, US03, US06 | Evento em múltiplos dias, administrador por setor e cadastro de convidados |
| Gabriel Lopes / Samara Alves / João Vitor Viana | 3 | US02, US04, US05 | Campos do evento, tradução automática e CRUD de líderes |
| Filipe Carvalho / Júlia Massuda / João Pedro Rodrigues | 1 | US05 | Migração do back-end para o CRUD de líderes |
| João Pedro Araújo / Riva Filho | 6 | US01 a US04, US08 | Garantia de qualidade, pipeline e testes de regressão |
| Daniel dos Santos Barros de Sousa / Luiz Henrique Soares | 2 | US01, US03 | Back-End de Evento em múltiplos dias e de administrador por setor |

---

## Detalhamento por história

| US | Assunto | Responsáveis | Status | Evidência |
|---|---|---|---|---|
| US01 | Evento com múltiplos dias, inclusive não consecutivos | Front: Davi Emanuel, Renan Vieira, João Pedro Ferreira <br> Back Daniel dos Santos Barros de Sousa, Luiz Henrique Soares | Concluído | Front: [PR #3](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/3) <br> Back: [PR #4](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Backend/pull/4), [PR #6](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Backend/pull/6) |
| US02 | Informações completas do evento | Gabriel Lopes, Samara Alves, João Vitor | Concluído | [commit 78de466](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/commit/78de466d82e35f9df0b49f09b1fd59b21fb8f2fc) |
| US03 | Administrador com acesso restrito ao setor | Front: Davi Emanuel, Renan Vieira, João Pedro Ferreira <br> Back:  Daniel dos Santos Barros de Sousa, Luiz Henrique Soares | Concluído | Front: [PR #3](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/3) <br> Back: [PR #5](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Backend/pull/5)|
| US04 | Página não traduzida automaticamente | Gabriel Lopes, Samara Alves, João Vitor | Em Revisão | [PR #6](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/6) |
| US05 | CRUD de líderes e galeria de diretores | Gabriel Lopes, Samara Alves, João Vitor, Filipe, Júlia, João Pedro Rodrigues | Em Andamento | [PR #2](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/2) |
| US06 | CRUD de bandas e palestrantes | Davi Emanuel, Renan Vieira, João Pedro Ferreira | Concluído | [PR #3](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/3) |

---

## Garantia de Qualidade

| ID | Entrega | Cobre | Responsável | Status |
|---|---|---|---|---|
| QA01 | Pipeline bloqueante no back-end | Todas | João Pedro Araújo | Em Revisão |
| QA02 | Pipeline de integração contínua no front-end | Todas | Riva Filho | Em Revisão |
| QA03 | Testes de regressão do idioma e da duplicação | US04, US08 | Riva Filho | Em Revisão |
| QA04 | Testes de evento com múltiplos dias | US01, US02 | João Pedro Araújo | Em Revisão |
| QA05 | Matriz de autorização e testes de negação de acesso | US03 | João Pedro Araújo | Em Revisão |
| QA06 | Templates de pull request e de relato de defeito | Processo | Riva Filho | Em Revisão |

**Resultados.** 60 testes novos, 12 testes quebrados corrigidos, 2 vulnerabilidades encontradas — uma corrigida, outra documentada na [Matriz de Autorização](matriz-autorizacao.md). Suíte do back-end: 631 testes, 95% de cobertura. O front-end passou a ter pipeline, executando ESLint, build e Playwright em toda pull request.

---

## Resumo

- *Líder da Apresentação*: a definir
- *Total de US concluídas:* 4 de 6 (US01, US02, US03 e US06)
- *US em revisão:* 1 (US04)
- *US em andamento:* 1 (US05)
- *Entregas de QA:* 6
- *Início da Sprint:* 01/09/2026
- *Fim da Sprint:* 14/09/2026

---

## Histórico de Versão

| Versão | Data | Descrição | Autor(es) |
|---|---|---|---|
| `1.0` | 14/09/2026 | Criação do relatório da Sprint 01 | [João Pedro Araújo (Jadequilin)](https://github.com/Jadequilin) |
