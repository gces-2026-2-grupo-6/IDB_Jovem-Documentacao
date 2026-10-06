# Sprint 02

*Período:* 15/09/2026 a 06/10/2026

## Entregas da Sprint

| Trio de Desenvolvedores | Qtd. de US | US | Assunto das US |
|---|---:|---|---|
| [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) / [Rivadalvio Joaquim da Silva Filho](https://github.com/RivaFilho) | 5 | US01, US03 a US06 | Garantia de qualidade: autorização, convidados, migrations e regressão das histórias de líderes |
| [Filipe Carvalho da Silva](https://github.com/Filipe-002) / [João Rodrigues](https://github.com/JpRodrigues2) / [Júlia dos Reis Teixeira Massuda](https://github.com/JuliaReis18) | 1 | US09 | Back-end da inscrição separada para voluntários |
| [Gabriel Lopes de Amorim](https://github.com/BrzGab) / [Maria Samara Alves Silva](https://github.com/SamaraAlvess) / [João Vitor Alves Viana](https://github.com/Joaovitor045) | 3 | US04, US05, US07 | Álbuns de fotos por evento (front-end) e envio das US04 e US05 para produção |

> **A preencher pelos trios:** as histórias novas assumidas na Sprint 2.

---

## Detalhamento por história

### US09 — Inscrição separada para voluntários (Back-end)

- **Requisito:** RF52
- **Descrição:** Eu, como *administradora*, desejo *disponibilizar dois links de inscrição por evento — um para participantes e outro para voluntários*, para *separar quem vai participar de quem vai trabalhar no evento*.
- **Responsáveis (Back-end):** [Filipe Carvalho da Silva](https://github.com/Filipe-002), [João Rodrigues](https://github.com/JpRodrigues2) e [Júlia dos Reis Teixeira Massuda](https://github.com/JuliaReis18).
- **Atuação:** implementação do back-end do fluxo de inscrição separada para voluntários na Sprint 2.


### US07 — Álbuns de fotos por evento (Front-end)

- **Requisitos:** RF07, RF23
- **Descrição:** Eu, como *administradora*, desejo *que as fotos fiquem organizadas em álbuns separados por evento*, para *que as imagens de eventos diferentes não se misturem*.
- **Responsáveis (Front-end):** [Gabriel Lopes de Amorim](https://github.com/BrzGab), [Maria Samara Alves Silva](https://github.com/SamaraAlvess) e [João Vitor Alves Viana](https://github.com/Joaovitor045).
- **Atuação:** a página pública `/galeria` deixou de mostrar uma grade única com as fotos de todos os eventos misturadas. Agora ela mostra um álbum por evento: cada card exibe a capa, o nome e o local do evento e a quantidade de fotos. Ao abrir um álbum, aparecem só as fotos daquele evento, e o botão de voltar retorna à lista de álbuns. As fotos vêm da galeria do Drive de cada evento que tem `link_galeria` cadastrado. Quando nenhum evento tem fotos, a página mostra quatro fotos de exemplo.
- **Código:** [PR #15](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/15) do front-end. O lint do CI falhava porque as imagens de exemplo eram referenciadas como texto (`"galeria1"`) e não pela variável importada, o que também impedia que aparecessem; corrigido no commit [087a130](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/commit/087a1300de7e646ec45dd6bdb9dfb72cfc523bea).
- **Critérios de aceitação:** atendidos o álbum próprio por evento e a separação das fotos entre eventos. Ficam para a próxima sprint a escolha da foto de capa (hoje é a primeira do álbum), a reordenação das imagens e a simplificação da inclusão de fotos pelo painel.
- **Próximos passos:** consumir o endpoint agregado `GET /evento/galerias/todas` do back-end da US07 ([PR #14](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Backend/pull/14) do back-end), que devolve as fotos de todos os eventos numa só chamada, e retirar as fotos de exemplo antes de enviar a página para produção.

### Envio das US04 e US05 para produção (Front-end)

- **Responsáveis:** [Gabriel Lopes de Amorim](https://github.com/BrzGab), [Maria Samara Alves Silva](https://github.com/SamaraAlvess) e [João Vitor Alves Viana](https://github.com/Joaovitor045).
- **Atuação:** as US04 e US05, concluídas na Sprint 1 no repositório do grupo, foram enviadas ao repositório da organização pela branch `deploy/us04-us05`, montada a partir da `main` da organização. O [PR #2 da organização](https://github.com/idbjovemnacional/IDB_Jovem-Teen/pull/2) foi aceito em 05/10/2026, e a Vercel publicou o site em produção no mesmo dia.
- **O que foi ao ar:**
    - **US04:** `lang="pt-BR"`, meta `notranslate` e `translate="no"` nos nomes dos líderes e no título da marca. O navegador do celular deixa de traduzir a página e a foto do Pr. Áquila não aparece mais duplicada.
    - **US05:** tela "Diretores & Líderes" no painel, restrita à superadministradora, e a seção "Nosso Organograma" da página inicial lendo os líderes da API.
- **Ajuste de contrato:** o painel passou a enviar `mini_biografia`, `redes_sociais` como objeto (`{"instagram": "@perfil"}`) e `gestao`, no formato do back-end da US05. Antes enviava `bio` e as redes como texto, o que daria erro 422 ao salvar um líder com rede social.
- **Sincronização dos repositórios do grupo:** o [PR #14](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/14) do front-end e o [PR #11](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Backend/pull/11) do back-end trouxeram o que foi enviado à produção de volta para a `main` do grupo, para que o PR com as demais histórias entre sem conflito. No back-end, a migration da US01 passou a partir da última migration da US05, evitando duas *heads* na cadeia (defeito registrado na QA09).
- **Verificação:** build ok e 253 testes Playwright passando no envio; após a sincronização, `npx eslint src` sem erros e 412 testes Playwright passando no front-end, e 668 testes passando no back-end.
- **Dependência:** a região, a mini-biografia, as redes sociais e a gestão do líder só são salvas depois que o back-end da US05 for publicado na VPS, onde o deploy não é automático (é preciso reconstruir o container e rodar `alembic upgrade head`).

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
| `1.3` | 05/10/2026 | Registro do trio no front-end da US07 e no envio das US04 e US05 para produção | [Gabriel Lopes de Amorim](https://github.com/BrzGab), [Maria Samara Alves Silva](https://github.com/SamaraAlvess) e [João Vitor Alves Viana](https://github.com/Joaovitor045) |
