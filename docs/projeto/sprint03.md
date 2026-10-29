# Sprint 03

*Período:* 07/10/2026 a 29/10/2026

## Entregas da Sprint

| Trio de Desenvolvedores | Qtd. de US | US | Assunto das US |
|---|---:|---|---|
| [Júlia dos Reis Teixeira Massuda](https://github.com/JuliaReis18), [Filipe Carvalho da Silva](https://github.com/Filipe-002) e [João Rodrigues](https://github.com/JpRodrigues2) | 3 | US10, US11, US14 | Back-end das três prioridades da cliente: cargo único e links fixados no painel, materiais para download e evento cancelado ou adiado |
| [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) / [Rivadalvio Joaquim da Silva Filho](https://github.com/RivaFilho) | 18 | US01, US02, US03, US04, US05, US06, US07, US08, US09, US10, US11, US14, US15, US18, US19, US20, US22, US23 e RNF05 | Garantia de qualidade: portões de CI, correção da listagem pública de eventos, contrato das respostas da API, cobertura das integrações com o Google e especificação executável das melhorias validadas com a cliente (US10, US11, US14, US19 e US23); no front-end, a suíte inteira de telas passou a rodar também em celular Android e iPhone — incluindo os testes das histórias entregues nesta sprint — e o painel administrativo foi verificado em tela pequena (RNF05) |
| [Gabriel Lopes de Amorim](https://github.com/BrzGab), [Maria Samara Alves Silva](https://github.com/SamaraAlvess) e [João Vitor Alves Viana](https://github.com/Joaovitor045) | 6 | US03, US18, US19, US20, US22, US23 | Front-end: separação das telas de produtos e eventos, exclusão de fotos do álbum, eventos online e presenciais, exclusão de evento com inscritos, missão, visão e valores e calendário com os eventos já ocorridos |
| [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206) e [Renan Vieira Guedes](https://github.com/R-enanVieira) | 3 | US10, US11, US14 | Front-end das três prioridades da cliente: links fixados no painel e recusa de cargo ocupado, área de materiais para download e aviso de evento cancelado ou adiado |
| [Luiz Henrique Guimarães Soares](https://github.com/luizh-gsoares), [Daniel dos Santos Barros de Sousa](https://github.com/daniel-de-sousa) | 4 | US18, US19, US20, US23 | Back-End: Exclusão de fotos do álbum, eventos online e presenciais, exclusão de evento com inscritos e calendário com os eventos já ocorridos.|

---

## Detalhamento por história

### US02 — Evento sem coordenadas derrubava a listagem pública

- **Requisito:** RF11
- **Responsável:** [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin)
- **Situação:** corrigido. Defeito registrado na Sprint 2 e previsto para esta sprint.

As colunas `local_latitude` e `local_longitude` aceitam nulo no banco, mas o
schema de resposta as exigia. O efeito não era um evento com problema: era
`GET /evento/` respondendo **500 inteiro**, porque a falha acontece na
serialização da resposta e vale para todos os itens da lista. Um único evento
sem coordenadas tirava a agenda do ar para todo visitante.

A correção tornou os dois campos opcionais apenas na resposta. O cadastro
continua exigindo as coordenadas (`SolicitacaoEvento`), então nada ficou mais
permissivo na entrada — e é justamente esse teste que vai acusar a mudança de
regra quando a [US19](backlog_melhorias.md#us19) entrar, porque o evento online
passa a ser aceito sem endereço.

O front já tratava latitude e longitude nulas antes de desenhar o mapa, o que
foi verificado antes da correção: a mudança não quebra tela nenhuma.

### US10 — Cadastro nacional de líderes (back-end)

- **Requisitos:** RF53, RF58
- **Responsáveis:** [Júlia dos Reis Teixeira Massuda](https://github.com/JuliaReis18), [Filipe Carvalho da Silva](https://github.com/Filipe-002) e [João Rodrigues](https://github.com/JpRodrigues2)
- **Situação:** back-end e front-end concluídos ([PR #16](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Backend/pull/16) e [Front-end](front_us10_us11_us14.md)).

O cadastro dos mais de 2500 líderes fica fora do site, num formulário
externo cujas respostas vão para uma planilha. O site só precisa fixar os
dois links na página inicial do painel e impedir que dois líderes ativos
ocupem o mesmo cargo. Foram duas entregas:

**Links fixados no painel.** `GET` e `PUT /painel/links` guardam o link de
cadastro e o da planilha (CA1, CA2 e CA4). As duas rotas são exclusivas da
superadministradora, **inclusive a leitura** (CA3): a planilha reúne nome,
e-mail e WhatsApp de mais de 2500 pessoas, e um link desses não pode chegar a
quem não deveria vê-lo. A atualização é parcial — trocar um link não apaga o
outro — e enviar o campo vazio remove o link.

**Cargo único por abrangência (RF58).** O cadastro e a edição de líder
recusam com **409** um segundo líder ativo no mesmo par (cargo, região), com
uma mensagem que diz quem ocupa o cargo e como liberá-lo (CA8). A regra
segue a decisão da cliente: "diretor regional do Sul" e "diretor regional do
Centro Oeste" são cargos diferentes e convivem (CA9). A comparação ignora
acento, caixa e hífen, para que "Vice-Diretor Nacional" e "vice diretor
nacional" não burlem a regra. Diretor anterior não ocupa cargo: marcar o
antecessor como anterior libera a vaga para o sucessor (CA7). A regra vale
também na edição, caminho que costuma escapar quando a validação entra só no
cadastro.

### US11 — Materiais para download (back-end)

- **Requisito:** RF54
- **Responsáveis:** [Júlia dos Reis Teixeira Massuda](https://github.com/JuliaReis18), [Filipe Carvalho da Silva](https://github.com/Filipe-002) e [João Rodrigues](https://github.com/JpRodrigues2)
- **Situação:** back-end e front-end concluídos ([PR #16](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Backend/pull/16) e [Front-end](front_us10_us11_us14.md)).

**Cadastro de materiais.** O módulo novo `material` guarda título, tipo
(foto, vídeo ou documento), descrição e o link do arquivo no Drive — o
arquivo continua no Drive, como a cliente decidiu, e o servidor não recebe
upload (CA1). A listagem é pública, sem login, com filtro por tipo (CA3), e
cadastrar, editar e excluir são exclusivos da superadministradora, recusados
pelo servidor para qualquer outro papel (CA2 e CA4).

**Download com o nome original.** A rota `GET /drive/download/{file_id}`
devolve o arquivo como anexo, com o nome que ele tem no Drive; documento
nativo do Google (Docs, Planilhas, Apresentações) é exportado em PDF. Como a
rota usa a conta institucional, ela **só entrega imagem ou arquivo cadastrado
como material**: o resto responde 404. Sem essa trava, quem soubesse o id de
um arquivo da conta — a planilha de líderes, por exemplo — conseguiria
baixá-lo sem login.

**Botão "Baixar" em todo o site (CA5).** Todo material e toda foto das
galerias (`/evento/{id}/galeria` e `/evento/galerias/todas`) passaram a trazer
`url_download`. O front não precisa montar URL nenhuma: o botão é um link
para esse campo.

### US14 — Marcar evento como cancelado ou adiado (back-end)

- **Requisito:** RF12
- **Responsáveis:** [Júlia dos Reis Teixeira Massuda](https://github.com/JuliaReis18), [Filipe Carvalho da Silva](https://github.com/Filipe-002) e [João Rodrigues](https://github.com/JpRodrigues2)
- **Situação:** back-end e front-end concluídos ([PR #16](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Backend/pull/16) e [Front-end](front_us10_us11_us14.md)).

O evento ganhou `situacao` (ativo, cancelado ou adiado) e `nova_data`,
opcional e válida só para o adiado (CA4). A marcação e o desfazer são feitos
por `PATCH /evento/{id}/situacao`, com um corpo de uma ou duas linhas,
liberado para o setor de eventos e para a superadministradora (CA1). Nada é
apagado — evento, inscrições e fotos continuam guardados (CA3) — e ninguém é
avisado, porque a comunicação com os inscritos segue sendo da equipe (CA5).

Na agenda do Google, o título ganha `[CANCELADO]` ou `[ADIADO]`. Se o Google
falhar, a marcação no site é mantida: a agenda é um espelho, e o site é a
fonte da verdade.

Um cuidado do PUT completo: um formulário que edita só a descrição de um
evento adiado, sem enviar a situação, **preserva** o adiamento e a nova data.
Voltar para ativo, ou passar a cancelado, limpa a nova data.


### Front-end das três prioridades (US10, US11 e US14)

- **Requisitos:** RF53, RF58, RF54 e RF12
- **Responsáveis:** [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206) e [Renan Vieira Guedes](https://github.com/R-enanVieira)
- **Situação:** concluído. Detalhamento e as 12 tarefas de cada história em [Front-end — US10, US11 e US14](front_us10_us11_us14.md).

As três histórias tinham o servidor pronto e nenhuma tela. O trio integrou as
rotas publicadas no [Guia do Front-end](guia_front_us10_us11_us14.md), sem
alterar o back-end.

**US10.** Os dois links passaram a ficar fixados na página inicial do painel,
com abrir, copiar e trocar — e, para quem não é superadministradora, o card não
aparece **e a rota não é chamada**: a planilha reúne dados pessoais de mais de
2500 líderes. A recusa 409 de cargo ocupado é exibida no próprio formulário,
preservando o que foi digitado, porque a correção é trocar o cargo ou marcar o
antecessor como diretor anterior.

**US11.** A área pública `/materiais` foi criada do zero, aberta sem login,
com filtro por tipo. O botão "Baixar" aponta para o `url_download` que o back-end
entrega pronto; link externo abre em nova aba, porque ali não há arquivo nosso
para entregar. O mesmo botão entrou em toda foto das galerias. O painel de
materiais é exclusivo da superadministradora.

**US14.** O aviso de cancelado ou adiado aparece nas quatro telas públicas, com
o texto decidido num lugar só, e os botões de inscrição somem enquanto durar a
situação. O evento marcado continua nas listas — esconder tiraria do visitante
justamente o aviso. No painel, a marcação é uma ação isolada, que salva sozinha
e não passa pelo formulário do evento.

**Verificação:** 46 testes E2E novos, suíte completa em 477 passando. O mock de
API ganhou as rotas novas, o `url_download` por foto e a recusa 409 por cargo
ocupado, de modo que a regra fica exercitada e não apenas descrita.

### Verificação das três histórias

- Os **8 testes `xfail`** da especificação executável (QA23) que estas
  histórias atendem — cargo único, download e situação do evento — passaram
  e tiveram o marcador removido, ficando como regressão. Restam 6, de
  US19, US23 e da exclusão com vínculo.
- **130 testes novos** no back-end: a suíte foi de 762 para 892 testes, com
  cobertura de 99,41% e pylint 9,88.
- A `MATRIZ` de autorização e o `CONTRATO` das respostas foram atualizados
  com as 9 rotas e os campos novos — ver a
  [Matriz de Autorização](matriz-autorizacao.md#sprint3).
- As três migrations ficaram encadeadas depois de `7a3d9e1c5b20`, com head
  único `d4a7b9c1e3f6`, e foram testadas em Postgres (upgrade, downgrade e
  upgrade).

### Front-end do Trio 01 — US03, US18, US19, US20, US22 e US23

- **Responsáveis:** [Gabriel Lopes de Amorim](https://github.com/BrzGab), [Maria Samara Alves Silva](https://github.com/SamaraAlvess) e [João Vitor Alves Viana](https://github.com/Joaovitor045)
- **Código:** [PR #27](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/27) (US19 e US20), [PR #28](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/28) (US22 e US23) e [PR #29](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/29) (US03 e US18) do front-end, todos mesclados na `main` do grupo.
- **Atuação:** o trio trabalhou em conjunto em todas as histórias, sem divisão por integrante. As seis foram feitas só no front-end; o back-end da US18, US19, US20 e US22 fica com outro trio, e o contrato esperado está descrito em cada história abaixo e nos PRs.

#### US19 — Eventos online e presenciais (front-end)

- **Requisito:** RF56
- **Situação:** front-end concluído ([PR #27](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/27)). back-end concluído ([17](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Backend/pull/17)).

O cadastro de evento ganhou a escolha entre **Presencial** e **Online** (CA1). No online, o mapa dá lugar ao campo do link da transmissão, que pode ser cadastrado depois; no presencial, o cadastro continua igual, com o local obrigatório. Os cards de `/eventos`, o destaque "Em breve" e a página do evento trazem um selo de modalidade, e um filtro **Modalidade** mostra só um dos tipos (CA5). A grade de `/eventos` passou a seguir a ordem das datas — antes seguia a ordem de cadastro —, que é o que o selo e o filtro preservam no lugar de duas seções separadas. O evento online mostra "Evento online" no lugar do endereço e o botão **Assistir à transmissão** quando há link; "Adicionar ao Google Calendar" leva o link como local. Evento sem o campo, como os já cadastrados, é tratado como presencial (CA4).

**Contrato esperado do back-end:** `modalidade` (`presencial` ou `online`) e `link_transmissao` no evento, com as coordenadas opcionais no online. Os dois testes `xfail` da QA23 sobre modalidade aceitam o nome `modalidade`.

#### US20 — Exclusão de evento com inscritos (front-end)

- **Requisito:** defeito QA08, RF13
- **Situação:** front-end concluído ([PR #27](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/27)). back-end concluído ([17](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Backend/pull/17)).

Antes de confirmar a exclusão, o modal mostra quantas inscrições de participantes e de voluntários, atividades da programação e vínculos com convidados saem junto com o evento, e avisa que o evento sai da agenda do Google (A1). A confirmação fica travada enquanto a contagem carrega e enquanto a exclusão roda, contra o clique duplo. Se a contagem falhar, o modal avisa e a exclusão continua possível; se o servidor responder que a agenda do Google falhou, a mensagem dele aparece e o evento continua na lista (A3).

**Contrato esperado do back-end:** `GET /evento/{id}/exclusao` devolvendo `participantes`, `voluntarios`, `atividades` e `convidados`; e a exclusão do evento apagando tudo isso junto, com a agenda só depois do banco (Opção A).

#### US22 — Missão, visão e valores (front-end)

- **Requisitos:** RF25. O RF26 (seção de avisos e comunicados na home), que o catálogo liga à mesma história, não foi tratado.
- **Situação:** front-end concluído ([PR #28](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/28)). Depende do back-end.

Nova página pública **Quem somos** (`/quem-somos`), com os blocos Missão, Visão e Valores; cada bloco só aparece quando tem conteúdo. O botão **Saiba mais** da seção "Conheça o IDB Jovem & Teen" da página inicial passou a levar à página — antes ia para o rodapé — e o rodapé ganhou o link "Missão, visão e valores" (navegação clara a partir da página inicial). No painel, a tela **Missão e valores**, só para a superadministradora, edita a missão, a visão e a lista de valores. A missão do rodapé passa a ser a mesma editada no painel.

**Contrato esperado do back-end:** `GET /institucional` (público) e `PUT /institucional` (só superadmin) com `{ missao, visao, valores: [] }`. Enquanto a rota não existir, o site mostra apenas a missão que já estava publicada no rodapé — nenhum texto inventado —, e salvar pelo painel avisa que o servidor ainda não oferece o cadastro.

#### US23 — Calendário com os eventos já ocorridos

- **Requisitos:** RF59, RF13
- **Situação:** concluída ([PR #28](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/28)).

O calendário da página inicial deixou de descartar os eventos encerrados: navegando para os meses anteriores, eles aparecem no dia em que ocorreram (CA1, CA2). O evento passado aparece em tons de cinza, com a etiqueta **Já aconteceu**, e leva à página do evento; com álbum, o botão vira **Ver fotos** (CA4). O evento em curso continua com a aparência normal, porque o que decide o passado é a data de término (CA3).

**Não dependeu do back-end.** O calendário da página inicial lê `GET /evento/`, que já devolve os eventos passados, e não o `ServicoCalendario`. O achado da QA23 sobre `timeMin=agora` continua valendo para a agenda do Google, mas não afeta este calendário.

#### US03 (revisão, CA1) — Produtos e eventos em telas separadas

- **Requisito:** RF01
- **Situação:** CA1 concluído ([PR #29](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/29)). CA2 e CA3 (setor Comercial no Keycloak) ficam para depois.

Eventos e produtos já tinham telas e itens de menu próprios; o único lugar que misturava os dois era a página inicial do painel, onde "Produtos Cadastrados" dividia a tela com o calendário de eventos. A página inicial passou a resumir só a agenda, e o resumo da loja é a própria tela **Produtos**. Quem administra apenas a loja entra direto na tela de Produtos, em vez de cair num painel sem nada do seu setor. Esta é a leitura da equipe para "separar a tela de produtos da tela de eventos".

#### US18 — Excluir fotos específicas de um álbum (front-end)

- **Requisitos:** RF55, RF07
- **Situação:** concluída — front-end no [PR #29](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/29) e back-end no [PR #18](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Backend/pull/18), mesclado em 28/10/2026.

Os detalhes do evento no painel ganharam a seção **Álbum do evento**, com as fotos e a capa marcada. A superadministradora seleciona uma ou mais fotos, ou todas, e confirma (CA1); o aviso diz que elas somem da galeria, da página do evento e de "Já aconteceram", que as outras fotos continuam e que o arquivo no Drive não é apagado (CA2, CA4, CA6). Excluída a capa, a foto seguinte assume (CA3). O administrador de setor vê o álbum, mas não a opção de excluir (CA5). As linhas de Próximos e Em Andamento da lista de eventos ganharam o atalho **Abrir evento e álbum**: até então, só os eventos anteriores tinham caminho até os detalhes, e um evento em andamento ficava sem acesso ao álbum pelo menu.

**Contrato esperado do back-end:** `POST /evento/{id}/galeria/excluidas` (só superadmin) com `{ "fotos": ["<id do arquivo>", ...] }`, e as fotos excluídas fora de `GET /evento/{id}/galeria` e de `GET /evento/galerias/todas`. Sem a rota, o álbum aparece normalmente e a exclusão avisa que o servidor ainda não oferece a função.


### Back-end do Trio 02 — US18, US19, US20 e US23

- **Responsáveis:** [Luiz Henrique Guimarães Soares](https://github.com/luizh-gsoares) e [Daniel dos Santos Barros de Sousa](https://github.com/daniel-de-sousa)
- **Código:** [PR #17](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Backend/pull/17) (US19 e US20) e [PR #18](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Backend/pull/18) (US18 e US23) do back-end.
- **Atuação:** a dupla trabalhou em conjunto nas quatro histórias, sem divisão por integrante. O back-end implementou os contratos consumidos pelo front-end; as telas não foram alteradas por esta frente.

#### US18 — Excluir fotos específicas de um álbum (back-end)
 
- **Requisitos:** RF55, RF07
- **Situação:** concluída no PR #18.
 
O back-end passou a persistir as fotos ocultadas de cada álbum na tabela `evento_foto_excluida` (chave composta `evento_id` e `foto_id`, com `ON DELETE CASCADE` para limpar os vínculos caso o evento seja removido). A rota `POST /evento/{evento_id}/galeria/excluidas`, exclusiva da superadministradora (CA5), valida o payload no schema `SolicitacaoFotosExcluidas` — removendo espaços em branco, descartando entradas vazias, deduplicando IDs na mesma requisição e recusando listas vazias com `422` —, registra apenas os IDs ainda não excluídos de forma idempotente e retorna `204 No Content` (CA1).
 
Na leitura pública, tanto `GET /evento/{evento_id}/galeria` quanto `GET /evento/galerias/todas` cruzam os arquivos retornados pelo Google Drive com os IDs registrados no banco e filtram as fotos excluídas antes de devolver a resposta (CA2). Como o site define a capa do álbum como a primeira foto da lista retornada, ocultar a foto de capa promove automaticamente a próxima foto disponível, e ocultar todas deixa o evento sem fotos públicas sem quebrar a listagem (CA3, CA4). Nenhuma chamada de exclusão é feita na API do Google Drive, preservando os arquivos originais na pasta institucional (CA6). Eventos inexistentes respondem `404`, e tentativas por administradores de setor ou usuários sem papel de superadmin são barradas com `403`.
 
**Contrato implementado:** `POST /evento/{evento_id}/galeria/excluidas` (`superadmin`) recebendo `{ "fotos": ["<file_id>", ...] }` → `204 No Content`, com filtragem automática dos IDs excluídos em `GET /evento/{evento_id}/galeria` e `GET /evento/galerias/todas`.

#### US19 — Eventos online e presenciais (back-end)

- **Requisito:** RF56
- **Situação:** concluída no PR #17, mesclado na `main`.

O evento passou a ter `modalidade` (`presencial` ou `online`) e `link_transmissao`. A migration `f1e2d3c4b5a6` cria as colunas com `modalidade` padrão `presencial`, cobrindo os eventos já cadastrados (CA4). No cadastro e na edição, evento presencial continua exigindo latitude e longitude e ignora o link; evento online dispensa coordenadas, limpa o endereço e usa o link da transmissão como local na agenda do Google. Eventos antigos, sem o campo, são lidos como presenciais. Os dois testes `xfail` da QA23 sobre modalidade passaram a rodar como testes normais.

**Contrato implementado:** `modalidade` e `link_transmissao` na resposta do evento, com coordenadas opcionais no online.

#### US20 — Exclusão de evento com inscritos (back-end)

- **Requisitos:** defeito QA08, RF13
- **Situação:** concluída no PR #17, mesclado na `main`.

A nova rota `GET /evento/{id}/exclusao`, restrita à superadministradora, devolve a contagem de participantes, voluntários, atividades e vínculos com convidados para o modal de confirmação (A1). O `DELETE /evento/{id}` passou a apagar o evento junto com as inscrições, os vínculos de voluntários, os vínculos com bandas/palestrantes e as atividades, em uma única transação (A2). Como as FKs de `trabalha`, `participa` e `atividade` não têm `ON DELETE CASCADE`, os vínculos são removidos explicitamente. O cadastro de participantes, voluntários e convidados é preservado; só o vínculo com o evento sai.

Para atender à atomicidade (A3), o banco prepara as exclusões e recebe `flush`, depois o evento é removido do Google Agenda e só então o banco recebe `commit`. Se o banco falhar, a agenda não é acionada; se o Google falhar, há `rollback`. Nesses casos a API responde `502` ou `500` com “Nada foi apagado”. Evento sem inscrições continua sendo excluído como antes.

**Contrato implementado:** `GET /evento/{id}/exclusao` (superadmin) e `DELETE /evento/{id}` (superadmin, `204` no sucesso). A US20 não exigiu migration.

#### US23 — Calendário com os eventos já ocorridos (back-end)
 
- **Requisitos:** RF59, RF13
- **Situação:** concluída no PR #18.
 
O `ServicoCalendario` deixou de filtrar os eventos pelo instante atual na consulta ao Google Calendar (`_montar_url_evento`). Antes, a URL era montada com `timeMin=datetime.now(timezone.utc).isoformat()` e `maxResults=10`, o que impedia qualquer evento encerrado de ser retornado pela API do Google e truncava a listagem em dez itens sem aviso (achado registrado pela QA23). O parâmetro `timeMin` foi removido para preservar o histórico de eventos já ocorridos (CA1 e CA2), e `maxResults` foi elevado para `250` com ordenação por `startTime`. Na conversão (`_converter_evento`), eventos passados continuam sendo transformados normalmente em `RespostaEvento`, e os dois testes de especificação que estavam como `xfail` em `TestCalendarioComEventosPassados` tiveram o marcador removido e passaram a rodar como regressão.
 
**Contrato implementado:** `ServicoCalendario._montar_url_evento()` consulta `GET /calendars/primary/events` sem `timeMin` e com `maxResults=250`, `singleEvents=true` e `orderBy=startTime`, retornando eventos passados, em curso e futuros em `listar_evento()`.

#### Verificação

- **76 testes novos** de Playwright: 32 da US19 e US20, 26 da US22 e US23 e 18 da US03 e US18. A suíte do front-end terminou a sprint com **514 testes passando** no modo da CI, contra o build, sem teste instável.
- `npx eslint src` sem erros em todos os commits, com os mesmos 15 avisos da `main`.
- Enquanto o back-end não entra, nenhuma tela quebra: os eventos aparecem como presenciais, os modais de exclusão avisam que não conseguiram conferir as inscrições ou que o servidor não oferece a função, e o site mantém a missão já publicada.

---

## Garantia de Qualidade

| ID | Entrega | Cobre | Responsável | Status |
|---|---|---|---|---|
| QA18 | Portões de cobertura e de lint na CI do back-end | Processo | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) | Concluído |
| QA19 | Correção do evento sem coordenadas na listagem pública | US02 | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) | Concluído |
| QA20 | Contrato das respostas consumidas pelo front fixado contra o OpenAPI | Processo | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) | Concluído |
| QA21 | Cobertura do token e dos retornos das chamadas ao Google | US07, US09 | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) | Concluído |
| QA22 | Busca de evento por nome verificada contra o Postgres | US01 | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) | Concluído |
| QA23 | Especificação executável das melhorias validadas com a cliente | US10, US11, US14, US19, US23 | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) | Concluído |
| QA24 | Testes end-to-end do front-end executados contra o build publicado | Processo | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) | Concluído |
| QA25 | Projetos móveis no Playwright: a suíte de telas rodando em Android e iPhone | RNF05 · toda a suíte de telas, e com ela todas as US com tela | [Rivadalvio Joaquim da Silva Filho](https://github.com/RivaFilho) | Concluído |
| QA31 | Painel administrativo verificado em tela pequena | RNF05 · US03, US05, US06, US09, US22 | [Rivadalvio Joaquim da Silva Filho](https://github.com/RivaFilho) | Concluído |

**Resultados.** 83 testes novos no back-end e 1 defeito de severidade alta
corrigido. A suíte foi de 725 para 808 testes coletados: 762 passam na suíte
rápida, 31 são de endpoint e rodam no job com Postgres, e 15 eram `xfailed` — 2
defeitos abertos da Sprint 2 e 13 regras então sem implementação, registradas de
propósito. Cobertura em 99,23% com piso de 97% exigido pela CI, e pylint em
9,86 com piso de 9,5. No front-end, os testes end-to-end passaram a rodar
contra o artefato publicado: 436 testes, sem falha e sem teste instável.

### QA18 — Portões de cobertura e de lint

A CI do back-end passava com qualquer cobertura: `pytest-cov` gerava o
relatório e ninguém o lia. O pylint exigia nota acima de 7,5 enquanto a nota
real do projeto era 9,88 — folga de 2,38 pontos em que a qualidade podia cair
sem que nada acusasse.

Agora a cobertura tem piso de 97% e o pylint, de 9,5. O piso do lint ficou
deliberadamente folgado: piso colado na nota do dia transforma qualquer merge
alheio em CI vermelha. A escolha se provou na mesma sprint — o merge do
`imagem_cache`, vindo da organização, levou a nota para 9,86 e a cobertura para
99,23%, e a CI seguiu verde porque havia margem.

Os workflows também passaram a rodar nas branches `feat/**`, `fix/**`, `qa/**`
e `chore/**`. Antes rodavam só em `main` e em pull request: quem empurrava uma
branch de trabalho não recebia retorno nenhum até abrir a PR.

### QA20 — Contrato das respostas

O front lê a resposta da API campo por campo: `evento.local_latitude`,
`lider.gestao`, `item.image`. Renomear ou remover um campo não quebrava teste
nenhum no back-end — quebrava a tela, em produção, sem aviso. Foi exatamente
assim que o defeito da QA19 passou: o contrato mudou de fato, de obrigatório
para nulo, sem ninguém registrar.

Os 8 schemas que o front consome passaram a ter campos e obrigatórios fixados
contra o OpenAPI publicado pela própria aplicação. Mudar o contrato continua
permitido; o que deixou de ser possível é mudá-lo em silêncio, porque a
alteração passa a exigir que alguém atualize a lista — e isso coloca a decisão
na revisão da PR. Um teste a mais garante que nenhuma rota seja publicada sem
declarar o formato da sua resposta.

### QA21 — Integrações com o Google

Esta entrega nasceu da revisão por mutação da Sprint 2 (QA17). Plantando
defeitos no código da galeria ([US07](backlog_melhorias.md#us07)) e da inscrição
por formulário ([US09](backlog_melhorias.md#us09)), **onze passaram sem nenhum
teste reclamar**, em dois grupos:

| Defeito plantado | Efeito em produção | Por que nenhum teste pegava |
|---|---|---|
| Remover o cabeçalho `Authorization` das chamadas ao Drive | O Google responde 401 e a galeria fica vazia — sem erro na tela, porque lista vazia é tratada como "evento sem fotos" | Os testes verificavam o resultado da listagem, nunca o que a requisição levava |
| Devolver `None` no id da pasta encontrada, no texto extraído da resposta do formulário e no evento buscado | Álbum some em silêncio; inscrição é descartada por "resposta incompleta" e o participante nunca aparece na lista | Os testes verificavam que o método era chamado, não o que ele devolvia |

Cobertura não mostra nada disso: as linhas rodavam em 99% dos casos. O que
faltava era asserção. Os 18 testes novos cobrem o token nas três chamadas ao
Drive, os quatro retornos, as URLs montadas e o mapa de perguntas do
formulário.

### QA22 — Busca contra o Postgres

`RepositorioEvento.pesquisar_por_nome` usa `similarity()`, da extensão
**pg_trgm** do Postgres. A suíte unitária roda em SQLite, onde essa função não
existe: a consulta aparece como coberta no relatório e **nunca é executada**.
Trocar o limiar, o operador ou o nome da coluna passava por todos os testes e
só quebraria na primeira busca em produção.

Os 15 testes novos exercitam a rota contra o Postgres de verdade, no job que já
sobe banco e aplicação em container: variações de grafia, curingas (`%%`, `__`,
aspas, tentativa de injeção) tratados como texto e o contrato de tamanho mínimo
do termo. Ficam marcados com `endpoint`, fora da suíte rápida.

### QA23 — Especificação executável das melhorias

As regras aceitas na reunião de 06/10/2026 entraram no
[backlog de melhorias](backlog_melhorias.md) e, quando esta entrega foi escrita,
ainda não tinham código. Em vez de esperar os trios para só então escrever
teste, cada regra foi registrada de uma das duas formas:

- **`xfail(strict=True)`** para o que o sistema ainda não faz. Hoje falha como
  esperado e não suja a suíte. No dia em que a implementação entrar, o pytest
  acusa `XPASS(strict)` e obriga quem implementou a remover o marcador,
  mantendo o teste como regressão. Se a entrega vier **diferente** do que a
  cliente pediu, o teste continua falhando e isso aparece na revisão da PR.
- **teste verde**, hoje, para o que o sistema já permite e não pode deixar de
  permitir. São a metade que importa: hoje não existe validação de cargo
  alguma, então "dois diretores regionais de regiões diferentes" já passa.
  Registrar isso agora é o que impede a implementação ingênua — unicidade só
  pelo nome do cargo — de entrar sem ninguém perceber
  ([RF58](backlog_melhorias.md#rf58), decisão explícita da cliente).

São 18 testes: 13 `xfail` e 5 verdes, cobrindo
[US19](backlog_melhorias.md#us19) (modalidade online e presencial),
[US14](backlog_melhorias.md#us14) (cancelado e adiado),
[US11](backlog_melhorias.md#us11) (download de material),
[US10](backlog_melhorias.md#us10)/[RF58](backlog_melhorias.md#rf58) (cargo único
por abrangência) e [US23](backlog_melhorias.md#us23)/[RF59](backlog_melhorias.md#rf59)
(calendário com eventos passados).

Nenhum nome de campo tinha sido combinado com os trios, então cada regra aceita
qualquer um dos nomes plausíveis. Quem implementasse com um nome fora da lista
acrescentaria o escolhido na própria PR — e a revisão ficaria sabendo que o
contrato público da API mudou.

**O que aconteceu depois (28/10/2026).** Os trios entregaram as cinco histórias
entre 26 e 28 de outubro — US10, US11 e US14 na
[PR #16](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Backend/pull/16), US19
e US20 na [PR #17](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Backend/pull/17),
US18 e US23 na [PR #18](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Backend/pull/18) —
e, ao fazê-lo, **removeram 12 dos 14 marcadores `xfail` do arquivo**, mantendo as
asserções intactas. As regras combinadas com a cliente passaram a ser testes de
regressão comuns, no mesmo commit que trouxe o código.

Vale registrar que os campos saíram com os nomes previstos — `modalidade`,
`situacao` e `url_download` —, então nenhuma regra precisou ser renegociada na
revisão. O ciclo completo, da anotação da reunião ao teste verde, foi: regra
registrada no backlog em 18/10, escrita como teste em 19/10, implementada e
convertida em regressão entre 26 e 28/10.

Ficaram fora de propósito os dois links fixados da US10: são conteúdo da página
inicial do painel, nenhuma rota os serve, e inventar uma API aqui amarraria o
trio a uma decisão que não é da equipe de QA. A verificação dessa regra é no
front-end.

### QA24 — End-to-end contra o build

O job de end-to-end do front-end subia o servidor de desenvolvimento e testava
contra ele. O Vite em modo dev serve os módulos separados, com recarregamento e
sem minificação — não é o que a Vercel publica. A suíte podia estar verde
enquanto o build real quebrava, e ninguém saberia antes de abrir o site
publicado.

O job de build passou a guardar o `dist/` como artefato e o de end-to-end a
baixar esse mesmo artefato antes de rodar. É o mesmo artefato em todo o
caminho: o que foi testado é o que vai para produção, não uma segunda
compilação feita na hora.

Na primeira execução contra o build, 11 testes falharam e 1 ficou instável.
Nenhum era defeito do site, e os dois achados valem por si:

**A suíte tem duas naturezas misturadas sob o mesmo comando.** Os 11 testes
fazem `page.evaluate(() => import('/src/services/eventService.js'))` — abrem o
navegador e chamam a função do serviço direto, com as rotas interceptadas. É
teste de unidade rodando dentro do Chromium, e depende do servidor de
desenvolvimento servir `src/` módulo por módulo. No build existe um bundle único
e minificado, então aquele caminho não existe: por construção eles não podem
rodar contra o artefato. O mesmo vale para a cobertura do
`vite-plugin-istanbul`, que só instrumenta em dev.

Em vez de desistir de testar o build, cada natureza passou a rodar onde
funciona, com dois projetos do Playwright: `build`, com 391 testes contra o
`dist` servido por `vite preview`, e `fonte`, com 45 testes contra o
`npm run dev`. A marcação é uma tag, aplicada aos 10 describes do arquivo que é
inteiro dessa natureza e a um único teste de outro arquivo — esse tem 22 testes
e só um importava módulo, então marcar o arquivo todo tiraria 21 testes de tela
justamente da verificação contra o build.

**Dois testes esperavam por tempo em vez de esperar pelo evento**, e perdem a
corrida em máquina carregada ou com um worker só, que é a condição da CI. Um
clicava em salvar e aguardava 600 ms antes de verificar se o alerta apareceu; o
outro dependia de a geolocalização responder dentro de 15 s, quando quem
responde é o tempo limite de 10 s do próprio navegador. Os dois passaram a
esperar pelo evento, verificados em repetição.

**Resultado:** 436 testes, sem falha e sem teste instável, com os dois
servidores de pé.

### QA25 — Projetos móveis no Playwright

- **Código:** [PR #22](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/22) do front-end, mesclado na `main` do grupo em 28/10/2026.

A suíte de telas só rodava num navegador de computador, e o site é usado
sobretudo pelo celular. O Playwright ganhou dois projetos novos,
`mobile-android` (Pixel 7, Chromium) e `mobile-iphone` (iPhone 13, WebKit),
que rodam a suíte inteira, como o desktop. O job de end-to-end da CI passou a
instalar também o WebKit, que o iPhone exige.

O que só faz sentido no computador foi pulado no celular, com a justificativa
no código:

- os testes que clicam na barra lateral ou na navegação do cabeçalho, que no
  celular ficam recolhidas atrás do botão "Abrir menu". O caminho pelo menu
  móvel é coberto pela QA31;
- 2 checagens de texto que o layout esconde de propósito no celular
  (`hidden sm:block`). Só a checagem é pulada; o resto do teste continua
  rodando.

**Os projetos móveis revelaram um defeito real:** no celular, as opções do
status do voluntário abrem fora da tela
([issue #24](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/issues/24)).
Os 3 testes afetados ficam suspensos com `test.fixme` só no Android, onde o
resultado varia entre execuções; o defeito é verificado de forma estável na
QA31.

Pôr o WebKit na CI expôs também instabilidades que não eram do site:

- a lista de `eventos-proximos.spec.js` era contada uma única vez, sem esperar
  o carregamento dos eventos;
- no WebKit, o primeiro toque no marcador do mapa às vezes é descartado pelo
  Leaflet e o popup não abre. O teste repete o toque até o popup aparecer;
- o `ubuntu-latest` passou a apontar para o 26.04, que o Playwright 1.60 não
  suporta: a instalação dos navegadores falhava e o job caía em menos de 30 s,
  antes de rodar qualquer teste. O job ficou fixado no Ubuntu 24.04;
- o `<Analytics />` do build busca `/_vercel/insights/script.js`, que só existe
  com o site hospedado na Vercel. Os testes passaram a responder esse script
  vazio, e o aviso deixou de encher o log da CI; o site em produção continua
  igual;
- o 404 proposital da página `/test-coverage` (voluntário 99999, para cobrir o
  caminho de erro) deixou de ser repetido no log. Qualquer outro 404 continua
  aparecendo.

### QA31 — Painel administrativo em tela pequena

- **Código:** [PR #23](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/23), mesclado na branch da QA25 e com ela na `main`.

Rodar a suíte existente no celular verifica que as telas funcionam, mas não
que cabem na tela. O arquivo novo `tests/admin/admin-mobile.spec.js` roda só
nos projetos móveis e verifica:

- o menu do painel começa fechado, abre pelo botão, leva a cada tela e fecha
  depois de navegar; o botão Sair é alcançável. A tela **Missão e valores** da
  US22 entrou aqui;
- 13 listagens e formulários do painel não rolam para o lado — nem a página,
  nem o `<main>`, que tem rolagem própria — e nenhum botão ou campo fica
  cortado;
- as ações de cadastro de cada listagem cabem inteiras na tela;
- as opções de status do voluntário podem ser tocadas.

Os defeitos confirmados ficam marcados com `test.fail`: o teste continua
rodando e fica vermelho quando o defeito for corrigido, avisando quem corrigiu
para retirar a marcação. Cada teste foi validado com uma quebra proposital,
desfeita antes do commit — menu desativado, botão mais largo que a tela e o
defeito do status corrigido temporariamente —, e falhou nos três casos.

### Defeitos e achados

| Defeito ou achado | Severidade | Situação |
|---|---|---|
| Evento sem latitude e longitude derruba a listagem inteira com 500, inclusive a pública. | Alta | Corrigido (QA19). Estava registrado desde a Sprint 2. |
| As chamadas ao Drive e ao Forms não tinham nenhum teste sobre o token nem sobre o que os métodos devolvem: 11 defeitos plantados passavam limpos. | Média | Corrigido (QA21) |
| A consulta de busca por nome nunca era executada em teste, por depender de função que só existe no Postgres. | Média | Corrigido (QA22) |
| O calendário descarta eventos passados **também no back-end**: `ServicoCalendario` monta a consulta ao Google com `timeMin=agora` e `maxResults=10`. | Média | Registrado (QA23). Interessa a quem pegar a US23: mexer apenas no filtro do front não trará o histórico. |
| A suíte do front-end misturava teste de unidade em navegador com teste de tela, e a CI verificava as duas contra o servidor de desenvolvimento. | Processo | Corrigido (QA24) |
| A CI não exigia piso de cobertura e exigia pylint 7,5 com nota real 9,88. | Processo | Corrigido (QA18) |
| O fork do grupo estava atrás da organização em duas migrations, o que geraria duas *heads* na primeira migration da sprint. | Processo | Corrigido: fork sincronizado, `alembic heads` em `7a3d9e1c5b20`. |
| No celular, as opções de status do voluntário (Pendente, Aprovado, Reprovado) abrem abaixo da área visível na última linha da tabela, e a administradora não consegue mudar o status. Causa provável: o `Dropdown` posiciona o menu abaixo do botão sem limitar à altura da janela. | Alta | Aberto ([issue #24](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/issues/24)). Coberto pela QA31 com `test.fail`. |
| Em `/admin/administradores`, no iPhone (390 px), o botão "Adicionar Administrador" passa 6 px da tela e o painel ganha rolagem para o lado. | Baixa | Aberto ([issue #25](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/issues/25)). Coberto pela QA31 com `test.fail`. |
| Tabela de voluntários ilegível no celular: as colunas de largura fixa deixam quase nenhum espaço para nome e e-mail, e não dá para saber de qual voluntário é cada linha. | Média | Aberto ([issue #26](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/issues/26)) |
| `ubuntu-latest` passou a apontar para uma versão que o Playwright não suporta, e o job de end-to-end caía antes de rodar qualquer teste. | Processo | Corrigido (QA25): job fixado no Ubuntu 24.04. |

### Ajuste em relação ao plano

O plano de QA previa começar a sprint pelos testes das histórias novas. Na
primeira metade da sprint os trios ainda não tinham entregue o código de US10,
US11, US14, US19 e US23, e a equipe optou por não ficar parada: as regras já
validadas com a cliente viraram especificação executável (QA23), e o esforço
restante foi para o que não depende de ninguém — os portões de CI, o defeito já
registrado, o contrato das respostas e as lacunas que a revisão por mutação da
Sprint 2 havia exposto.

A escolha se pagou na segunda metade. Entre 26 e 28 de outubro as entregas
chegaram em bloco — sete histórias no back-end e nove no front-end — e a
verificação não precisou ser escrita do zero: os testes já existiam, e os trios
converteram 12 dos 14 `xfail` em regressão no mesmo commit que trouxe o código.
Os projetos móveis da QA25, mesclados em 28/10, já rodaram sobre essas
histórias novas.

Para a Sprint 4 fica o que a entrega em bloco deixou em aberto: o back-end da
US22 e os CA2 e CA3 da US03 — e, do lado da QA, os testes de tela das histórias
que acabaram de se completar. A Sprint 5 segue reservada só para o que depender
de código que ainda não existe.

---

## Resumo

- *US completas nas duas pontas:* 7 — US10, US11, US14, US18, US19, US20 e US23
- *US entregues em uma das pontas:* 2 — a US22, com o front-end pronto ([PR #28](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/28)) e o back-end pendente; e a US03, com o CA1 concluído ([PR #29](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/29)) e os CA2 e CA3 fora desta sprint
- *Trabalho dos trios por ponta:* 7 histórias no back-end — US10, US11 e US14 ([PR #16](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Backend/pull/16)); US19 e US20 ([PR #17](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Backend/pull/17)); US18 e US23 ([PR #18](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Backend/pull/18)) — e 9 no front-end: US10, US11 e US14 pelo Trio 02, e US03 (CA1), US18, US19, US20, US22 e US23 pelo Trio 01
- *Histórias cobertas pela QA:* 18 e o RNF05 — US01, US02, US07, US09, US10, US11, US14, US19 e US23 no back-end; e, no front-end, toda história com tela, porque a suíte inteira passou a rodar também em Android e iPhone: US01 a US09, US15, US18, US19, US20, US22 e US23
- *Entregas de QA:* 9 — QA18 a QA23 no back-end e na sua esteira e QA24 no pipeline do front-end, por [João Pedro](https://github.com/Jadequilin); QA25 e QA31 nos testes de tela em celular, por [Rivadalvio](https://github.com/RivaFilho)
- *Líder da Apresentação*: Gabriel Lopes de Amorim
- *Início da Sprint:* 07/10/2026
- *Fim da Sprint:* 29/10/2026

---

## Histórico de Versão

| Versão | Data | Descrição | Autor(es) |
|---|---|---|---|
| `1.0` | 19/10/2026 | Criação do relatório da Sprint 03 com as entregas de QA do back-end e do pipeline (QA18 a QA24), a correção do defeito da listagem pública e os achados da sprint | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) |
| `1.1` | 26/10/2026 | Registro do back-end das US10, US11 e US14 pelo trio, com a verificação das três histórias e o guia do front-end | [Júlia dos Reis Teixeira Massuda](https://github.com/JuliaReis18), [Filipe Carvalho da Silva](https://github.com/Filipe-002) e [João Rodrigues](https://github.com/JpRodrigues2) |
| `1.2` | 27/10/2026 | Registro do front-end das US03 (CA1), US18, US19, US20, US22 e US23 pelo Trio 01, com o contrato esperado do back-end em cada história | [Gabriel Lopes de Amorim](https://github.com/BrzGab), [Maria Samara Alves Silva](https://github.com/SamaraAlvess) e [João Vitor Alves Viana](https://github.com/Joaovitor045) |
| `1.3` | 28/10/2026 | Registro das entregas de QA do front-end em tela pequena (QA25 e QA31): projetos móveis no Playwright, verificação do painel no celular e os três defeitos encontrados | [Rivadalvio Joaquim da Silva Filho](https://github.com/RivaFilho) |
| `1.4` | 28/10/2026 | Registro da dupla do Back-end | [Daniel dos Santos Barros de Sousa](https://github.com/daniel-de-sousa) |
| `1.5` | 28/10/2026 | Fechamento do relatório da Sprint 3: correção dos números da QA18 a QA24 (83 testes novos, não 64; 15 na QA22); registro de que os trios converteram 12 dos 14 `xfail` da QA23 em regressão ao entregar as histórias; dupla de QA reunida numa linha, como nas Sprints 1 e 2, com as 18 histórias que a suíte passou a cobrir em celular; resumo refeito (7 histórias completas nas duas pontas); data de fim corrigida para 29/10/2026; e a situação da US18 no front-end deixando de dizer que depende do back-end, já entregue | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) |
| `1.6` | 28/10/2026 | Registro do líder da apresentação da Sprint 3 | [Gabriel Lopes de Amorim](https://github.com/BrzGab) |
