# Sprint 02

*Período:* 15/09/2026 a 28/09/2026

## Entregas da Sprint

| Trio de Desenvolvedores | Qtd. de US | US | Assunto das US |
|---|---:|---|---|

| [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) / [Rivadalvio Joaquim da Silva Filho](https://github.com/RivaFilho) | 5 | US01, US03 a US06 | Garantia de qualidade: autorização, convidados, migrations e regressão das histórias de líderes |

> **A preencher pelos trios:** as histórias novas assumidas na Sprint 2.

---

## Detalhamento por história


---

## Garantia de Qualidade

| ID | Entrega | Cobre | Responsável | Status |
|---|---|---|---|---|
| QA07 | Testes do vínculo entre evento e convidado e da edição de convidados | US06 | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) | Em Revisão |
| QA08 | Registro do defeito na exclusão de evento ou convidado com vínculo | US06 | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) | Em Revisão |
| QA09 | Verificação de *head* único na cadeia de migrations | US01, US05 | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) | Em Revisão |
| QA10 | Proteção da listagem de inscritos e matriz de autorização lida do código | US03 | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) | Em Revisão |
| QA11 | Recusa de token com papéis em formato inesperado nas duas guardas de acesso | US03 | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) | Em Revisão |
| QA12 | Remoção da sobreposição com os testes de idioma da PR #6 | US04 | [Rivadalvio Joaquim da Silva Filho](https://github.com/RivaFilho) | A Fazer |
| QA13 | Testes da tela de líderes no painel | US05 | [Rivadalvio Joaquim da Silva Filho](https://github.com/RivaFilho) | A Fazer |
| QA14 | Testes da galeria de diretores restrita ao cargo nacional | US05 | [Rivadalvio Joaquim da Silva Filho](https://github.com/RivaFilho) | A Fazer |
| QA15 | Testes da tela de convidados com busca por função | US06 | [Rivadalvio Joaquim da Silva Filho](https://github.com/RivaFilho) | A Fazer |
| QA16 | Issue para as regras de `react-hooks` rebaixadas a aviso | Processo | [Rivadalvio Joaquim da Silva Filho](https://github.com/RivaFilho) | A Fazer |

**Resultados.** 94 testes novos no back-end, 5 defeitos encontrados — 3 corrigidos e 2 registrados para a equipe de implementação. Suíte do back-end: de 610 para 747 testes (43 da reintegração da QA05 e 94 desta sprint), cobertura de 95,3% para 97,2%. `evento` passou de 90,0% para 99,6% e `banda_palestrante` de 93,1% para 100%.

**Defeitos encontrados.**

| Defeito | Severidade | Situação |
|---|---|---|
| A listagem de inscritos de cada evento era pública: devolvia nome e e-mail, enumeráveis pelo id, e gravava no banco a cada chamada. | Alta | Corrigido (QA10) |
| Excluir um evento ou convidado com vínculo responde 500 e não apaga nada. No evento, a agenda do Google é apagada antes do banco recusar. | Alta | Registrado (QA08). A correção depende de decisão: apagar os vínculos junto ou recusar com 409. |
| As migrations da US05 e da US01 partem da mesma revisão; ao mesclar a US05, `alembic upgrade head` falha. | Alta | Registrado (QA09). A US05 deve apontar o `down_revision` para `259093bc4c8e`. |
| A guarda de setor aceitava papéis em formato de dicionário e respondia 500 com papéis nulos. | Média | Corrigido (QA11) |
| A correção e os testes da QA05 não chegaram à `main`: a PR foi mesclada numa branch empilhada já integrada. | Processo | Corrigido. PRs de QA passam a ter base sempre na `main`. |

**Ajuste em relação ao plano.** O plano de QA previa cobrir o serviço e o repositório de líderes. Ao iniciar a sprint, a equipe da US05 já tinha levado `src/lider` a 100% na própria branch. O esforço foi redirecionado para a autorização (antecipada da Sprint 4) e para as migrations, motivadas pelo conflito da US05.

---

## Resumo

- *Líder da Apresentação*: a definir
- *Total de US concluídas:* a definir
- *Entregas de QA:* 10 (5 do back-end em revisão, 5 do front-end a fazer)
- *Início da Sprint:* 15/09/2026
- *Fim da Sprint:* 28/09/2026

---

## Histórico de Versão

| Versão | Data | Descrição | Autor(es) |
|---|---|---|---|
| `1.0` | 21/09/2026 | Criação do relatório da Sprint 02 com as entregas de QA do back-end | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) |
