# Sprint 02

*Período:* 15/09/2026 a 06/10/2026

## Entregas da Sprint

| Trio de Desenvolvedores | Qtd. de US | US | Assunto das US |
|---|---:|---|---|
| [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) / [Rivadalvio Joaquim da Silva Filho](https://github.com/RivaFilho) | 5 | US01, US03 a US06 | Garantia de qualidade: autorização, convidados, migrations e regressão das histórias de líderes |
| [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr) / [Renan Vieira Guedes](https://github.com/R-enanVieira) / [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206) | 2 | US09, US15 | Front-end da inscrição separada para voluntários; histórico de eventos passados com fotos |
| [Filipe Carvalho da Silva](https://github.com/Filipe-002) / [João Rodrigues](https://github.com/JpRodrigues2) / [Júlia dos Reis Teixeira Massuda](https://github.com/JuliaReis18) | 1 | US09 | Back-end da inscrição separada para voluntários |
| [Gabriel Lopes de Amorim](https://github.com/BrzGab) / [Maria Samara Alves Silva](https://github.com/SamaraAlvess) / [João Vitor Alves Viana](https://github.com/Joaovitor045) | 3 | US04, US05, US07 | Álbuns de fotos por evento (front-end) e envio das US04 e US05 para produção |
| **Back-end:** [Luiz Henrique Guimarães Soares](https://github.com/luizh-gsoares), [Daniel dos Santos Barros de Sousa](https://github.com/daniel-de-sousa) | 1 | US07 | Experiência do Usuário e Engajamento - Gestão de fotos de eventos |



---

## Detalhamento por história

### US09 — Inscrição separada para voluntários (Back-end)

- **Requisito:** RF52
- **Descrição:** Eu, como *administradora*, desejo *disponibilizar dois links de inscrição por evento — um para participantes e outro para voluntários*, para *separar quem vai participar de quem vai trabalhar no evento*.
- **Responsáveis (Back-end):** [Filipe Carvalho da Silva](https://github.com/Filipe-002), [João Rodrigues](https://github.com/JpRodrigues2) e [Júlia dos Reis Teixeira Massuda](https://github.com/JuliaReis18).
- **Atuação:** implementação do back-end do fluxo de inscrição separada para voluntários na Sprint 2.

### US09 — Inscrição separada para voluntários (Front-end)

- **Requisito:** RF52
- **Responsáveis (Front-end):** [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [Renan Vieira Guedes](https://github.com/R-enanVieira) e [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206).
- **Situação:** concluído e integrado com o back-end. [PR #16](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/16) do repositório do front-end.

**O que foi entregue.** O evento deixou de ter uma inscrição e passou a ter duas, independentes entre si: participantes (quem vai ao evento) e voluntários (quem trabalha nele). Até aqui existia um `formulario_link` só, usado pelo voluntariado — e o botão "Inscreva-se" das telas públicas abria justamente esse formulário, de modo que quem só queria ir ao evento acabava preenchendo a ficha de quem vai trabalhar nele.

No painel, o formulário do evento ganhou dois campos de link, e a tela de inscrições virou uma casca com duas abas, uma por fluxo, cada uma com a sua tabela, o seu link e a sua contagem. A listagem de voluntários mantém pendente/aprovado/reprovado; a de participantes não tem status, porque ninguém aprova quem vai ao evento — é o que justifica tabelas separadas em vez de um filtro sobre a mesma lista.

Nas telas públicas, "Inscreva-se" passou a apontar para a inscrição de participante e "Seja Voluntário" para a de voluntário, na página do evento, nos cards da listagem e no evento em destaque. Cada botão só aparece quando o evento abriu aquele fluxo: antes "Seja Voluntário" era exibido em todo evento e não fazia nada nos que não tinham formulário.

**Divisão do trabalho.** A mesma lógica das US01, US03 e US06: o contrato e a regra num módulo isolado com Davi Emanuel, o painel administrativo com João Pedro, e as telas públicas e a verificação com Renan Vieira.

**Verificação.** 16 testes E2E novos: as duas listagens, a separação entre elas, os dois links, os estados de fluxo não aberto, a degradação quando a API não fornece a listagem e o erro real de servidor.

**Integração com o back-end.** O front foi escrito antes de o back-end expor o fluxo de participantes, sobre nomes supostos. Com a entrega do trio do back-end, os nomes foram alinhados ao contrato publicado:

| | Suposto pelo front | Publicado pelo back-end |
|---|---|---|
| Campo do evento | `formulario_link_participantes` | `formulario_participante_link` |
| Listagem | `/formulario/eventos/{id}/inscricoes-participantes` | `/formulario/eventos/{id}/participantes` |
| Id do inscrito | `inscricao_id` | `participante_id` |

A troca custou três linhas, porque os nomes estavam isolados na camada de serviço desde o início — foi justamente o que esse isolamento preparou. As duas listagens passaram a exigir o setor Inscrições, inclusive a de voluntários, que era pública: o cliente HTTP do front já manda o token em toda chamada, então o painel segue funcionando. **Não restou pendência de integração nesta história.**

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

### US15 — Histórico de eventos passados com fotos (Front-end)

- **Requisito:** RF13
- **Responsáveis:** [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [Renan Vieira Guedes](https://github.com/R-enanVieira) e [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206).
- **Situação:** concluído. [PR #20](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/20) do repositório do front-end.

**Por que entrou agora.** A história estava no backlog como Prioridade Baixa, com a nota "depende de US07". A US07 foi mesclada nesta sprint, e com isso a US15 deixou de estar bloqueada — foi o que permitiu pegá-la sem depender de mais ninguém.

**O que foi entregue.** Um evento encerrado sumia do site público inteiro: a agenda, o mapa e a página inicial filtram todos pelo que ainda não terminou. As fotos existiam na galeria, mas soltas do evento que as originou — não havia como ver o que já aconteceu.

A página de eventos ganhou a seção "Já aconteceram", depois da agenda, com os eventos encerrados do mais recente para o mais antigo. Cada card traz a capa do álbum, a contagem de fotos, a data e o local, e leva à página do evento, onde o álbum da US07 já é exibido.

| Decisão | Motivo |
|---|---|
| A capa é a primeira foto do álbum; sem álbum, a imagem do evento | Mantém o card completo mesmo para evento antigo sem galeria cadastrada |
| Se a foto do Drive falhar, troca pela imagem do evento — uma vez só | Evita card com capa quebrada, e a marca impede laço se a reserva também falhar |
| A contagem de fotos só aparece quando há fotos | Evento sem álbum não deve prometer o que não tem |
| A seção se esconde sozinha sem eventos encerrados, e também se a busca falhar | O histórico é complemento da agenda; não pode derrubar o resto da página |
| Só busca a galeria de quem tem `linkGaleria` | Evita uma chamada por evento sem necessidade |

**Sem dependência do back-end.** Os eventos já vinham de `GET /evento/` e as fotos de `GET /evento/{id}/galeria`, ambos já consumidos pela aplicação. Nenhuma alteração foi pedida à outra equipe.

**Verificação.** 5 testes E2E: a ordenação do mais recente para o mais antigo, o selo de fotos só onde há álbum, o link para a página do evento, a separação entre histórico e agenda, e a seção sumindo quando não há evento encerrado.

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
- *Total de US concluídas:* 12
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
| `1.4` | 05/10/2026 | Registro da entrega do front-end da US09, com a divisão em doze tasks e a integração com o contrato publicado pelo back-end | [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [Renan Vieira Guedes](https://github.com/R-enanVieira) e [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206) |
| `1.5` | 05/10/2026 | Adição de contribuição da Sprint 02 | [Daniel dos Santos Barros de Sousa](https://github.com/daniel-de-sousa) |
| `1.6` | 05/10/2026 | Registro da entrega da US15 — histórico de eventos passados com fotos | [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [Renan Vieira Guedes](https://github.com/R-enanVieira) e [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206) |
| `1.7` | 05/10/2026 | Divisão do trabalho da US09 dita em uma linha, no lugar da referência à tabela de tasks removida | [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [Renan Vieira Guedes](https://github.com/R-enanVieira) e [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206) |
