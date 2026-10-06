# Sprint 02

*Período:* 15/09/2026 a 06/10/2026

## Entregas da Sprint

| Trio de Desenvolvedores | Qtd. de US | US | Assunto das US |
|---|---:|---|---|
| [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) / [Rivadalvio Joaquim da Silva Filho](https://github.com/RivaFilho) | 5 | US01, US03 a US06 | Garantia de qualidade: autorização, convidados, migrations e regressão das histórias de líderes |
| [Filipe Carvalho da Silva](https://github.com/Filipe-002) / [João Rodrigues](https://github.com/JpRodrigues2) / [Júlia dos Reis Teixeira Massuda](https://github.com/JuliaReis18) | 1 | US09 | Back-end da inscrição separada para voluntários |
| **Back-end:** [Luiz Henrique Guimarães Soares](https://github.com/luizh-gsoares), [Daniel dos Santos Barros de Sousa](https://github.com/daniel-de-sousa) | 1 | US07 | Experiência do Usuário e Engajamento - Gestão de fotos de eventos |

> **A preencher pelos trios:** as histórias novas assumidas na Sprint 2.

---

## Detalhamento por história

### US09 — Inscrição separada para voluntários (Back-end)

- **Requisito:** RF52
- **Descrição:** Eu, como *administradora*, desejo *disponibilizar dois links de inscrição por evento — um para participantes e outro para voluntários*, para *separar quem vai participar de quem vai trabalhar no evento*.
- **Responsáveis (Back-end):** [Filipe Carvalho da Silva](https://github.com/Filipe-002), [João Rodrigues](https://github.com/JpRodrigues2) e [Júlia dos Reis Teixeira Massuda](https://github.com/JuliaReis18).
- **Atuação:** implementação do back-end do fluxo de inscrição separada para voluntários na Sprint 2.


---

## Garantia de Qualidade

| ID | Entrega | Cobre | Responsável | Status |
|---|---|---|---|---|
| QA07 | Testes do vínculo entre evento e convidado e da edição de convidados | US06 | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) | Concluído |
| QA08 | Registro do defeito na exclusão de evento ou convidado com vínculo | US06 | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) | Concluído |
| QA09 | Verificação de *head* único na cadeia de migrations | US01, US05 | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) | Concluído |
| QA10 | Proteção da listagem de inscritos e matriz de autorização lida do código | US03 | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) | Concluído|
| QA11 | Recusa de token com papéis em formato inesperado nas duas guardas de acesso | US03 | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) | Concluído |
| QA12 | Remoção da sobreposição com os testes de idioma da PR #6 | US04 | [Rivadalvio Joaquim da Silva Filho](https://github.com/RivaFilho) | Concluído|
| QA13 | Testes da tela de líderes no painel | US05 | [Rivadalvio Joaquim da Silva Filho](https://github.com/RivaFilho) | Concluído|
| QA14 | Testes da galeria de diretores restrita ao cargo nacional | US05 | [Rivadalvio Joaquim da Silva Filho](https://github.com/RivaFilho) | Concluído |
| QA15 | Testes da separação de convidados por função na página pública | US06 | [Rivadalvio Joaquim da Silva Filho](https://github.com/RivaFilho) | Concluído |
| QA16 | Issue para as regras de `react-hooks` rebaixadas a aviso | Processo | [Rivadalvio Joaquim da Silva Filho](https://github.com/RivaFilho) | Concluído|
| QA17 | Revisão da suíte do back-end por teste de mutação | Processo | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) | Concluído |

**Resultados.** 94 testes novos no back-end e 4 defeitos encontrados no código — 2 corrigidos e 2 registrados para a equipe de implementação. A suíte foi de 610 para 747 testes (43 da reintegração da QA05 e 94 desta sprint) e, após a revisão da QA17, para 581, mantendo a mesma detecção de defeitos. Cobertura de 95,3% para 97,2%: `evento` passou de 90,0% para 99,6% e `banda_palestrante` de 93,1% para 100%.

**Revisão da suíte (QA17).** Cada teste foi avaliado por teste de mutação: 622 defeitos plantados no código, registrando quais testes pegavam cada um. Um arquivo só foi removido se, sem ele, nenhum defeito deixasse de ser pego. Saíram 166 casos — uma cópia literal de seis arquivos, duas suítes do mesmo serviço, cenários testados em duas camadas, repetições com valores equivalentes e a matriz manual da Sprint 1. Os 519 defeitos que a suíte pegava continuam sendo pegos, com a mesma cobertura. Um teste antigo não tinha nenhum `assert`; foi substituído por um que confere o resultado.

**Defeitos encontrados.**

| Defeito | Severidade | Situação |
|---|---|---|
| A listagem de inscritos de cada evento era pública: devolvia nome e e-mail, enumeráveis pelo id, e gravava no banco a cada chamada. | Alta | Corrigido (QA10) |
| Excluir um evento ou convidado com vínculo responde 500 e não apaga nada. No evento, a agenda do Google é apagada antes do banco recusar. | Alta | Registrado (QA08). A correção depende de decisão: apagar os vínculos junto ou recusar com 409. |
| As migrations da US05 e da US01 partiam da mesma revisão; ao mesclar a US05, `alembic upgrade head` falhava. | Alta | Corrigido na PR #11, encadeando a migration da US01 depois das da US05. Como a reordenação deixa sem as colunas novas todo banco já atualizado antes, uma migration de reparo cria o que faltar sem afetar quem já está correto. |
| Evento sem latitude e longitude derruba a listagem inteira com erro 500, inclusive a pública: a coluna aceita nulo, mas o schema de resposta exige os dois. | Média | Registrado. Previsto para a Sprint 3. |
| A guarda de setor aceitava papéis em formato de dicionário e respondia 500 com papéis nulos. | Média | Corrigido (QA11) |
| A correção e os testes da QA05 não chegaram à `main`: a PR foi mesclada numa branch empilhada já integrada. | Processo | Corrigido. PRs de QA passam a ter base sempre na `main`. |

**Ajuste em relação ao plano.** O plano de QA previa cobrir o serviço e o repositório de líderes. Ao iniciar a sprint, a equipe da US05 já tinha levado `src/lider` a 100% na própria branch. O esforço foi redirecionado para a autorização (antecipada da Sprint 4) e para as migrations, motivadas pelo conflito da US05.

---

## Resumo

- *Líder da Apresentação*: João Pedro Araújo de Freitas Lyra
- *Total de US concluídas:* a definir
- *Entregas de QA:* 11 (6 do back-end e 5 do front-end)
- *Início da Sprint:* 15/09/2026
- *Fim da Sprint:* 06/10/2026

---

## Histórico de Versão

| Versão | Data | Descrição | Autor(es) |
|---|---|---|---|
| `1.0` | 21/09/2026 | Criação do relatório da Sprint 02 com as entregas de QA do back-end | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) |
| `1.1` | 05/10/2026 | Registro do trio no back-end da US09 da Sprint 2 | [Filipe Carvalho da Silva](https://github.com/Filipe-002), [João Rodrigues](https://github.com/JpRodrigues2) e [Júlia dos Reis Teixeira Massuda](https://github.com/JuliaReis18) |
| `1.2` | 05/10/2026 | Registro da QA17, dos números após a revisão da suíte e da situação dos defeitos de migration e de coordenadas | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) |
| `1.3` | 05/10/2026 | Adição de contribuição da Sprint 02 | [Daniel dos Santos Barros de Sousa](https://github.com/daniel-de-sousa) |
