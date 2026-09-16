# Sprint 01

*Período:* 01/09/2026 a 14/09/2026

## Entregas da Sprint

| Trio de Desenvolvedores | Qtd. de US | US | Assunto das US |
|---|---:|---|---|
| [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr) / [Renan Vieira Guedes](https://github.com/R-enanVieira) / [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206) | 3 | US01, US03, US06 | Evento em múltiplos dias, administrador por setor e cadastro de convidados (front-end) |
| [Gabriel Lopes de Amorim](https://github.com/BrzGab) / [Maria Samara Alves Silva](https://github.com/SamaraAlvess) / [João Vitor Alves Viana](https://github.com/Joaovitor045) | 3 | US02, US04, US05 | Campos do evento, tradução automática e CRUD de líderes (front-end) |
| [Luiz Henrique Guimarães Soares](https://github.com/luizh-gsoares) / [Daniel dos Santos Barros de Sousa](https://github.com/daniel-de-sousa) | 2 | US01, US03 | Back-end das histórias de evento e de controle de acesso |
| [Filipe Carvalho da Silva](https://github.com/Filipe-002) / [João Rodrigues](https://github.com/JpRodrigues2) / [Júlia dos Reis Teixeira Massuda](https://github.com/JuliaReis18) | 1 | US05 | Persistência dos campos de líderes no back-end |
| [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) / [Rivadalvio Joaquim da Silva Filho](https://github.com/RivaFilho) | 5 | US01 a US04, US08 | Garantia de qualidade, pipeline e testes de regressão |

---

## Detalhamento por história

| US | Assunto | Responsáveis | Status |
|---|---|---|---|
| US01 | Evento com múltiplos dias, inclusive não consecutivos | **Front-end:** [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [Renan Vieira Guedes](https://github.com/R-enanVieira), [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206)<br>**Back-end:** [Daniel dos Santos Barros de Sousa](https://github.com/daniel-de-sousa), [Luiz Henrique Guimarães Soares](https://github.com/luizh-gsoares) | Concluído |
| US02 | Informações completas do evento | [Gabriel Lopes de Amorim](https://github.com/BrzGab), [Maria Samara Alves Silva](https://github.com/SamaraAlvess), [João Vitor Alves Viana](https://github.com/Joaovitor045) | Concluído |
| US03 | Administrador com acesso restrito ao setor | **Front-end:** [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [Renan Vieira Guedes](https://github.com/R-enanVieira), [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206)<br>**Back-end:** [Luiz Henrique Guimarães Soares](https://github.com/luizh-gsoares), [Daniel dos Santos Barros de Sousa](https://github.com/daniel-de-sousa) | Concluído |
| US04 | Página não traduzida automaticamente pelo navegador | [Gabriel Lopes de Amorim](https://github.com/BrzGab), [Maria Samara Alves Silva](https://github.com/SamaraAlvess), [João Vitor Alves Viana](https://github.com/Joaovitor045) | Concluído |
| US05 | CRUD de líderes e galeria de diretores anteriores | **Front-end:** [Gabriel Lopes de Amorim](https://github.com/BrzGab), [Maria Samara Alves Silva](https://github.com/SamaraAlvess), [João Vitor Alves Viana](https://github.com/Joaovitor045)<br>**Back-end:** [Filipe Carvalho da Silva](https://github.com/Filipe-002), [João Rodrigues](https://github.com/JpRodrigues2), [Júlia dos Reis Teixeira Massuda](https://github.com/JuliaReis18) | Concluído |
| US06 | CRUD de bandas e palestrantes | [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [Renan Vieira Guedes](https://github.com/R-enanVieira), [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206) | Concluído |

---

## Garantia de Qualidade

| ID | Entrega | Cobre | Responsável | Status |
|---|---|---|---|---|
| QA01 | Pipeline bloqueante no back-end | Todas | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) | Concluído |
| QA02 | Pipeline de integração contínua no front-end | Todas | [Rivadalvio Joaquim da Silva Filho](https://github.com/RivaFilho) |Concluído |
| QA03 | Testes de regressão do idioma e da duplicação de líder | US04, US08 | [Rivadalvio Joaquim da Silva Filho](https://github.com/RivaFilho) | Concluído |
| QA04 | Testes de evento com múltiplos dias e dias não consecutivos | US01, US02 | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) | Concluído |
| QA05 | Matriz de autorização e testes de negação de acesso | US03 | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) | Concluído |
| QA06 | Templates de pull request e de relato de defeito | Processo | [Rivadalvio Joaquim da Silva Filho](https://github.com/RivaFilho) | Concluído |

**Resultados.** 60 testes novos, 12 testes quebrados corrigidos e 2 vulnerabilidades encontradas — uma corrigida, outra documentada na [Matriz de Autorização](matriz-autorizacao.md). Suíte do back-end: 631 testes, 95% de cobertura. O front-end passou a ter pipeline, executando ESLint, build e Playwright em toda pull request.

---

## Resumo

- *Líder da Apresentação*: a definir
- *Total de US concluídas:* 5 de 6 (US01, US02, US03, US05 e US06)
- *US em revisão:* 1 (US04)
- *Entregas de QA:* 6
- *Início da Sprint:* 01/09/2026
- *Fim da Sprint:* 14/09/2026

---

## Histórico de Versão

| Versão | Data | Descrição | Autor(es) |
|---|---|---|---|
| `1.0` | 14/09/2026 | Criação do relatório da Sprint 01 | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) |
| `1.1` | 15/09/2026 | Altera o registro dos nomes dos membros do grupo, afim de facilitar a busca | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin)| 
