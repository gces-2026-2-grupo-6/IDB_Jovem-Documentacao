# Front-end — US10, US11 e US14

**Sprint:** 3 · **Repositório:** [IDB_Jovem-Teen](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen)
**Responsáveis:** [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206) e [Renan Vieira Guedes](https://github.com/R-enanVieira)
**Base:** [Guia do Front-end](guia_front_us10_us11_us14.md), publicado pelo trio do back-end na [PR #16](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Backend/pull/16)

Este documento registra a entrega do front-end das três prioridades da cliente.
O back-end foi concluído na Sprint 3 e nada nele foi alterado: a equipe do
front lê o código do servidor apenas para conferir contrato.

## Resumo da entrega

| História | O que faltava | O que o front entregou | Critérios |
|---|---|---|---|
| [US10](backlog_melhorias.md#us10) | Os dois links fixados e a recusa de cargo ocupado não tinham tela | Card na página inicial do painel, com abrir, copiar e trocar; mensagem do 409 no formulário de líder | CA1 a CA4, CA8 e CA9 |
| [US11](backlog_melhorias.md#us11) | A área de materiais não existia no sistema | Página pública `/materiais`, painel restrito à superadministradora e botão "Baixar" em toda foto | CA1 a CA5 |
| [US14](backlog_melhorias.md#us14) | A situação do evento não aparecia nem escondia a inscrição | Aviso nas quatro telas públicas e marcação no painel | CA1 a CA4 |

**Verificação:** 46 testes E2E novos (Playwright), com a suíte completa em 477
testes passando. O mock de API dos testes ganhou as rotas `/painel/links`,
`/material/` e `PATCH /evento/{id}/situacao`, além do `url_download` nas fotos
e da recusa 409 por cargo ocupado.

---

## US10 — Cadastro nacional de líderes

O cadastro dos mais de 2500 líderes acontece **fora do site**: cada líder se
inscreve por um formulário externo e as respostas caem numa planilha. Ao site
cabem duas coisas — fixar os dois endereços para a superadministradora e
impedir que dois líderes ativos ocupem o mesmo cargo.

**Os dois links (CA1 a CA4).** O card fica na página inicial do painel, com o
link de cadastro (para divulgar) separado do link da planilha (para consultar).
Cada um pode ser aberto, copiado e trocado ali mesmo, sem a equipe técnica. A
atualização é parcial, então salvar um link não apaga o outro, e o campo em
branco remove o link fixado.

Para o administrador de setor (CA3) o card não aparece **e a rota não é
chamada**. Esconder não bastaria: a planilha reúne nome, e-mail e WhatsApp de
mais de 2500 pessoas, e o back-end recusa a leitura com 403 justamente por
isso. O teste verifica as duas coisas — que o card some e que nenhuma
requisição a `/painel/links` sai do navegador.

**Cargo ocupado (CA8 e CA9).** A recusa 409 do servidor já vem com a frase
pronta, dizendo quem ocupa o cargo e como liberá-lo. O front a exibe como está,
**dentro do formulário** e não num alerta: a correção costuma ser trocar o
cargo ou marcar o antecessor como diretor anterior, e um alerta que some
apagaria o que a pessoa acabou de digitar. A regra vale também na edição — o
caminho que costuma escapar quando a validação entra só no cadastro.

### As 12 tarefas

| # | Tarefa | Critério |
|---:|---|---|
| 1 | `services/api/painelApi.js` — `GET` e `PUT /painel/links` | CA1, CA2 |
| 2 | `services/painelService.js` — adaptação dos campos e validação do endereço | CA4 |
| 3 | Tratamento de 403 e 422 com mensagem própria | CA3 |
| 4 | Componente `LinksLideresCard` na página inicial do painel | CA1, CA2 |
| 5 | Card do link de cadastro, com abrir e copiar | CA1 |
| 6 | Card da planilha, separado do de cadastro | CA2 |
| 7 | Estado "Configurar" quando o link ainda não existe | CA2 |
| 8 | Edição do endereço no próprio card, com salvar e cancelar | CA4 |
| 9 | Não chamar a rota quando a pessoa não é superadministradora | CA3 |
| 10 | 409 de cargo ocupado tratado no `liderService` | CA8 |
| 11 | Recusa exibida no formulário, preservando o que foi digitado | CA8, CA9 |
| 12 | 11 testes E2E, incluindo a troca de mandato (CA7) | CA1–CA4, CA7–CA9 |

---

## US11 — Materiais para download

A área de materiais **não existia** no sistema. O arquivo continua no Drive: o
site guarda o cadastro e o link, e nunca recebe upload.

**Área pública (CA1, CA3).** `/materiais` é aberta a qualquer visitante — a
rota não exige login e a tela não fala com o contexto de autenticação. Cada
material aparece identificado pelo tipo, com filtro por foto, vídeo ou
documento, e o visitante chega lá pelo menu da página inicial.

**O botão "Baixar" (CA5).** O endereço vem pronto do back-end em
`url_download`; o front não monta URL do Drive. Arquivo do Drive volta como
anexo, com o nome original e sem pedir login. Link externo — um vídeo no
YouTube, por exemplo — volta como está, e aí o botão **abre em nova aba** em
vez de baixar: não há arquivo nosso para entregar, e forçar o download daria em
nada. O mesmo botão foi colocado em toda foto das galerias, do evento e da
galeria pública, que passaram a trazer `url_download` por foto.

**Painel (CA2, CA4).** Cadastrar, editar e excluir são exclusivos da
superadministradora. A rota exige o papel, o menu não oferece o item e o
servidor recusa quem insistir. Excluir remove o cadastro e o material sai da
área pública; o arquivo continua no Drive, e a confirmação diz isso para não
deixar dúvida na hora de clicar.

### As 12 tarefas

| # | Tarefa | Critério |
|---:|---|---|
| 1 | `services/api/materialApi.js` — listar, criar, editar e excluir | CA1, CA4 |
| 2 | `services/materialService.js` — adaptação e validação dos campos | CA1 |
| 3 | Tratamento de 401, 403, 404 e 422 | CA2 |
| 4 | Página pública `/materiais`, sem login | CA3 |
| 5 | Filtro por tipo: foto, vídeo e documento | CA1 |
| 6 | Card do material identificado pelo tipo | CA1 |
| 7 | Botão "Baixar" apontando para `url_download` | CA5 |
| 8 | Link externo abre em nova aba em vez de baixar | CA5 |
| 9 | Item "Materiais" no menu do site, a partir da home | CA3 |
| 10 | Painel `/admin/materiais` restrito à superadministradora | CA2 |
| 11 | `MaterialForm` para cadastro e edição, e exclusão com confirmação | CA1, CA4 |
| 12 | Botão "Baixar" nas galerias + 16 testes E2E | CA5 |

---

## US14 — Evento cancelado ou adiado

O evento marcado **continua aparecendo** no site. Esconder o evento tiraria do
visitante justamente o aviso que a história quer dar: o que muda é que a
situação é informada e a inscrição some enquanto ela durar.

**Nas telas públicas (CA2, CA4).** O aviso aparece na agenda, no calendário da
página inicial, no carrossel da home e na página do evento. O texto é decidido
num lugar só — "Evento cancelado", "Adiado para 10/04/2027" ou "Evento adiado —
nova data em breve" — para que as quatro telas digam a mesma coisa. Os botões
de inscrição, participante e voluntário, só existem no evento ativo; "Veja
mais" continua, porque é na página do evento que o visitante entende o que
aconteceu.

**No painel (CA1, CA3).** A marcação fica **fora** do formulário do evento, e
salva sozinha. São ações de natureza diferente: editar reescreve tudo o que
está no formulário, enquanto marcar a situação é uma decisão isolada, que vale
na hora. Separadas, cancelar um evento não exige revalidar coordenadas, datas e
links. A nova data só é pedida no adiado, é opcional, e voltar para "ativo"
desfaz a marcação.

Nada é apagado (CA3) e ninguém é avisado (CA5): a comunicação com os inscritos
continua sendo feita pela equipe, por fora. Um cuidado verificado por teste: o
formulário completo não envia `situacao`, e o servidor só sobrescreve o que
recebe — então editar a descrição de um evento adiado **preserva** o
adiamento.

### As 12 tarefas

| # | Tarefa | Critério |
|---:|---|---|
| 1 | `situacao` e `nova_data` mapeados na adaptação do evento | CA2 |
| 2 | Constante `SITUACAO` e os predicados de cancelado e adiado | CA2 |
| 3 | `mostrarInscricao`, que decide a exibição dos botões | CA4 |
| 4 | `avisoDeSituacao`, com o texto do aviso num lugar só | CA2 |
| 5 | `handleAlterarSituacaoEvento` — `PATCH /evento/{id}/situacao` | CA1 |
| 6 | Componentes `EventStatusNotice` (faixa) e `EventStatusBadge` (selo) | CA2 |
| 7 | Aviso na página do evento | CA2 |
| 8 | Aviso e selo na agenda, sem filtrar o evento da lista | CA2, CA4 |
| 9 | Aviso no calendário da página inicial | CA2 |
| 10 | Aviso no carrossel da página inicial | CA2 |
| 11 | `SituacaoEventoCard` no painel: marcar, adiar e desfazer | CA1, CA3 |
| 12 | Selo na lista do painel + 19 testes E2E | CA1–CA4 |

---

## Decisões de implementação

**O front não monta URL de download.** Tanto o material quanto a foto trazem
`url_download` pronto do back-end. Montar a URL no cliente duplicaria uma regra
que já mora no servidor e quebraria no dia em que o caminho mudasse.

**Esconder botão não é controle de acesso.** As telas restritas escondem o que
a pessoa não pode fazer e, nos casos em que o dado é sensível, nem chegam a
pedi-lo. A barreira que vale continua sendo a do servidor; o front evita
oferecer o que será recusado.

**A recusa aparece onde a correção acontece.** O 409 de cargo ocupado e os
erros de material são exibidos no próprio formulário, com os dados preservados,
em vez de num alerta que some.

## Histórico de Versão

| Versão | Data | Descrição | Autor(es) |
|---|---|---|---|
| `1.0` | 28/10/2026 | Registro da entrega do front-end das US10, US11 e US14, com o detalhamento das 12 tarefas de cada história | [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206) e [Renan Vieira Guedes](https://github.com/R-enanVieira) |
