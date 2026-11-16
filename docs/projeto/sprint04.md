# Sprint 04

*Período:* 30/10/2026 a 19/11/2026

## Entregas da Sprint

| Trio de Desenvolvedores | Qtd. de US | US | Assunto das US |
|---|---:|---|---|
| [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) / [Rivadalvio Joaquim da Silva Filho](https://github.com/RivaFilho) | 5 | US01, US02, US11, US14 e US19 | Garantia de qualidade: testes com Selenium contra o que o servidor entrega e contra a produção publicada, três amarrações de Selenium no pipeline, duas sessões de testes exploratórios com seis achados e a correção dos dois defeitos de severidade alta encontrados |
| [Gabriel Lopes de Amorim](https://github.com/BrzGab) | 10 | US02, US03, US04, US05, US07, US18, US19, US20, US22 e US23 | Testes funcionais com Selenium sobre a pilha real — front logado pelo Keycloak, back-end e banco de verdade —, com ambiente em contêiner e três defeitos de integração registrados ([PR #31](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/31)) |
| [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206) e [Renan Vieira Guedes](https://github.com/R-enanVieira) | 7 | RF51, US13, US16, US08, US02, US09 e US14 | Front-end: histórico de participações por convidado, duplicação de evento, acessibilidade e leitor de tela, e card de compartilhamento por evento. Mais três correções de defeitos que estavam em produção: campos do evento descartados em silêncio, opções de status inalcançáveis no celular e a página inicial anunciando como "próximo" um evento já encerrado |

> As demais linhas desta tabela são preenchidas por cada trio ao registrar sua
> entrega, como nas sprints anteriores.

---

## Detalhamento por história

### QA32 — Selenium: o que o servidor entrega e o que está publicado

- **Responsável:** [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin)
- **Situação:** concluída.

O professor acrescentou Selenium como exigência da sprint. O caminho óbvio
seria reimplementar em Selenium o que a suíte Playwright já faz — 436 testes
refeitos para fazer as mesmas perguntas, com resultado idêntico e manutenção
dobrada. A pergunta útil é outra: **o que a suíte atual, por construção, não
consegue verificar?** São três coisas, e as três são reais.

**A suíte Playwright simula a rede.** Todo teste de tela passa por
`tests/helpers/apiMock.js`. Nenhum teste de tela do projeto fala com a API de
verdade. Se o contrato da API mudar, a simulação continua devolvendo o formato
antigo e a suíte continua verde.

**Nada olhava para o que está publicado.** A suíte roda contra um build
servido localmente. Entre 17/10 e 10/11 o Keycloak ficou fora do ar em
produção — **vinte e quatro dias** — e nenhuma verificação percebeu. O site
público continuou respondendo; só o login estava morto. A suíte estava verde e
estava certa: ela não fala com a produção.

**É uma pilha de automação só.** Defeito escondido por configuração de uma
ferramenta não aparece nela. Uma segunda pilha é verificação cruzada de graça.

Os testes ficaram dentro da pasta `selenium/`, no padrão da
[PR #31](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/31), em vez
de abrir uma segunda suíte no mesmo repositório.

**`testes/test_entrega_do_build.py` (7 testes e 1 especificação executável).**
Cada teste entra pela URL, como quem recebeu um link pronto — e esse caminho
depende de configuração de servidor, que é justamente o que muda entre o build
local e a produção. Verifica: rota profunda entrando direto sem cair no 404 do
servidor (a regra de `rewrites` do `vercel.json`); o 404 da aplicação para rota
inexistente, que é o inverso — o fallback não pode engolir tudo em silêncio;
navegação pelo menu sem recarregar a página inteira; o painel exigindo sessão
com o navegador limpo, e não com o estado de autenticação montado por *helper*;
e `lang` e `meta viewport` do `index.html` publicado, que nenhum teste de tela
percebe se o build perder, mas leitor de tela e celular percebem.

**`testes/test_producao.py` (6 testes).** Exigem a pilha publicada no ar e só
rodam com `ALVO=producao`. Verificam o endpoint de descoberta do realm — o
incidente dos vinte e quatro dias, virado teste —, as rotas públicas da API, o
CORS liberando o domínio do site, os campos das migrações da Sprint 3 presentes
no contrato publicado, os eventos saindo da API até a tela e o login real de
ponta a ponta.

Dois deles não usam navegador, de propósito. O navegador esconde justamente o
detalhe que importa: resposta bloqueada por CORS chega na tela como lista
vazia, não como erro — parece que não há evento cadastrado. E a verificação das
migrações existe porque *deploy* com `git pull` e `build` mas sem
`alembic upgrade head` deixa o código novo falando com o banco velho; o sintoma
é 500 na primeira consulta que toca a coluna nova, nunca um erro de build.

**A especificação executável**, no formato da Sprint 3 (`xfail` estrito): a
rota `/test-coverage` é o arnês que a suíte Playwright usa para medir cobertura
e hoje vai para produção, publicando cinco serviços em `window` — entre eles
`window.api`, o cliente HTTP já configurado com o interceptador que anexa o
token da sessão, numa rota pública. O peso no *bundle* foi medido e é pequeno
(956,57 kB para 943,22 kB), então o argumento é exposição, não tamanho. O
conserto não é apagar a página, que a suíte Playwright usa: é condicionar a
rota a `import.meta.env` para que ela não entre no build de produção.

### QA33 — Selenium no pipeline

- **Responsável:** [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin)
- **Situação:** concluída.

Três amarrações:

**`selenium-entrega`** consome o mesmo artefato `dist` que o job de E2E, para
verificar o que o servidor entrega.

**`selenium-funcional`** liga a suíte da [PR #31](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/31)
ao CI. Ela entrou sem job, e suíte que ninguém executa fica verde por ausência,
não por acerto — apodrece em duas sprints. O job faz *checkout* do back-end (os
dois repositórios são públicos, o `GITHUB_TOKEN` padrão basta), sobe a pilha
pelo `subir.sh`, serve o front e roda os testes funcionais, guardando os
*prints* das falhas como artefato.

**`verificacao-producao.yml`** roda contra o que está publicado: à mão, pelo
botão *Run workflow*, depois de cada *deploy*, e sozinho toda segunda às 9h17
de Brasília — para que uma queda silenciosa apareça em dias, não em semanas.

Os dois módulos novos levam o marcador `fora_do_ambiente`, porque não precisam
de banco, Keycloak nem back-end. Isso exigiu uma mudança pequena no
`conftest.py` da suíte funcional: a verificação de ambiente chama `pytest.exit`,
que derruba a sessão inteira, e passou a acontecer apenas quando algum teste
funcional foi coletado.

**Deploy automático do front, religado.** O `vercel.json` do repositório
carregava `"git": { "deploymentEnabled": false }`, que desliga o deploy
automático **do repositório inteiro**, não de uma branch. Era a causa de o
envio para produção de 10/11 ter precisado de `npx vercel --prod` na mão, e de
nenhum merge na `main` publicar nada desde então. O bloco saiu; se um dia for
preciso desligar o deploy só numa branch, o lugar certo é o *Ignored Build
Step* do Vercel, com `VERCEL_GIT_COMMIT_REF`. O `.gitignore` ganhou junto
`.vercel` e `.env*`.

Firefox ficou fora do CI por ora, de propósito. A suíte roda nele com
`NAVEGADOR=firefox`, e é o único navegador que a Playwright nunca cobre
(Chromium e WebKit). Portão que nunca passou uma vez fica vermelho na primeira
*pull request* de outra pessoa, e aí alguém desliga o portão — foi assim que o
piso de cobertura quebrou a pipeline na Sprint 3. Entra depois de uma execução
manual limpa.

### QA34 — Testes exploratórios: duas sessões, seis achados

- **Requisitos tocados:** RF01, RF12, RF46, RF56 e RNF08
- **Responsável:** [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin)
- **Situação:** concluída.

A suíte do back-end tem 928 testes e 98,77% de cobertura. Ela responde bem a
*o que já sabemos que pode quebrar continua funcionando?* e não responde a *o
que ainda não pensamos em perguntar?*. Todo teste automatizado é uma pergunta
escrita no passado: o defeito que ninguém previu passa por ele sem encostar — e
passa com a cobertura marcando 100% naquela linha, porque a linha executou, só
não foi interrogada.

**Os seis achados desta sprint estão todos em linhas com cobertura,
exercitadas por testes que passam.** É a demonstração mais direta de que
cobertura mede execução, não verificação.

**Método.** Sessão com carta (missão de uma frase, não roteiro), tempo fechado
de 60 minutos, registro durante e não depois, um oráculo por observação — o
critério que diz que aquilo é defeito — e todo achado confirmado virando teste.
Achado que não vira teste some na próxima sprint.

#### Sessão EX-01 — superfície de autenticação e integração com o Google

| Achado | Severidade | Desfecho |
|---|---|---|
| `GET /auth/callback` devolve o *refresh token* do Google no corpo da resposta | Alta | **corrigido** |
| O mesmo *refresh token* é impresso no log do contêiner | Alta | **corrigido** |
| Administrador sem papel de setor alcança todos os setores | Média, operacional | registrado, teste verde |

O `access_token` expira em uma hora; o `refresh_token` não expira sozinho —
quem o obtiver fala com a conta Google do projeto (Drive, Calendar, Forms) até
alguém revogar, e ninguém vai revogar, porque não há sinal de que vazou. Agrava
que o *callback* é um `GET`: a URL com o `code` já circula por histórico de
navegador, log de *proxy* e `Referer`. A origem está na própria resposta —
`"instrucoes": "Copie o refresh_token do terminal/tela"` —, conveniência de
desenvolvimento que foi para produção.

E há um detalhe que explica por que a cobertura não bastava: **existia um teste
no repositório que afirmava que os tokens voltam na resposta**
(`test_auth_controller.py::test_callback_google_sucesso`). Quem o escreveu
descreveu o comportamento observado, não o desejado.

**Correção.** Apagar os dois caminhos quebraria quem configura o servidor, que
precisa do token uma vez para preencher `GOOGLE_REFRESH_TOKEN`. Ele passou a
ser gravado num arquivo dentro do contêiner, com permissão 600, e a resposta diz
o comando para lê-lo e a instrução de apagá-lo em seguida. A resposta não
carrega mais token nenhum — nem o *refresh*, nem o *access* —, e o `print` saiu.
Os dois `xfail` viraram testes verdes: especificação cumprida vira regressão. O
teste que afirmava o contrário mudou junto.

De quebra, a resposta passou a distinguir o caso em que o Google não devolve
*refresh token*: ele só vem no primeiro consentimento, e o silêncio anterior já
custou tempo de quem configurou o servidor.

**O terceiro achado está certo hoje**: a regra está comentada em
`src/security.py` como retrocompatibilidade, para não trancar do lado de fora
quem já era administrador antes da US03. O risco é operacional e cresceu nesta
sprint, porque a cliente passou a criar administradores sozinha pelo console do
Keycloak: criar o usuário com o papel `admin` e esquecer o papel de setor não
produz um administrador limitado — produz um com acesso a tudo, sem nenhum
aviso na tela. O teste verde trava o comportamento para que fechar essa porta
seja um ato deliberado, e não um efeito colateral.

#### Sessão EX-02 — o que o cadastro aceita gravar

Carta: o que o sistema aceita gravar nas histórias entregues na Sprint 3,
procurando estado que o cadastro permite e que a tela não tem como representar.

| Achado | Severidade | Desfecho |
|---|---|---|
| US14: dá para adiar um evento para uma data **no passado** | Média | `xfail` estrito |
| US19: evento online é aceito **sem link de transmissão** | Média | `xfail` estrito |
| US19: `link_transmissao` não tem validação nenhuma de formato | Média | `xfail` estrito |

"Adiado para 10/03/2020" não descreve nada: o visitante não sabe se o evento já
aconteceu, se foi cancelado ou se alguém errou a digitação. O projeto já recusa
a incoerência irmã — `validar_datas` barra data de fim antes da data de início
—, e esta ficou de fora.

O CA5 da US19 diz que o link de transmissão é o que o evento online guarda "no
lugar do endereço", e o endereço é obrigatório no presencial. A simetria não foi
implementada: o presencial sem coordenadas é recusado com mensagem explícita, o
online sem link passa, e o card mostra um selo "Online" que não leva a lugar
nenhum. O formato também não é verificado, enquanto o link do material (US11)
passa por `_link_http` — **a mesma equipe validou um e não o outro**. A regra
que existe hoje mora só no formulário do front: quem chama a API direto grava
qualquer texto.

Os três ficaram como especificação executável, com a correção proposta escrita
no motivo, e **não** como mudança de comportamento. O motivo é concreto: validar
entrada que o front já envia muda o contrato entre as duas pontas, e verificar
isso exige a pilha integrada de pé. São regras de produto dos trios, diferente
dos dois achados da EX-01, que eram vazamento de credencial.

#### Um achado sem teste, por ora

**O evento adiado some da agenda pública quando a data original passa.** O
filtro `isOngoingOrFuture` usa `endDate || date` e ignora `nova_data` —
inclusive na branch da [PR #30](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/30).
Isso contraria o CA4 da US14, que pede que o evento continue aparecendo com o
aviso. O teste depende da PR #30 entrar.

### Revisão da entrega de front-end das US10, US11 e US14

- **Responsável pela revisão:** [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin)
- **Situação:** revisada, correções solicitadas.

A [PR #30](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/30) está
aberta desde 29/10 com o E2E vermelho: **28 testes falhando**, em quatro
causas. Nenhuma delas é defeito do produto; a última não foi possível
descartar.

As duas primeiras são a mesma coisa técnica: testes que fazem
`page.evaluate(() => import('/src/services/...'))`, o que só existe no servidor
de desenvolvimento — no projeto `build` o `/src/` não é servido. Em
`tests/public/us14-evento-cancelado-adiado.spec.js` (21 falhas) a correção não
é marcar com `@fonte`, porque são testes do que o visitante vê e precisam rodar
contra o build: o caminho é o `setupApiMock` já nascer com o evento marcado. Em
`tests/admin/us11-materiais.spec.js` (3 falhas) `@fonte` **é** a correção
certa, porque o teste chama o serviço de propósito para exercitar a validação
de quem contorna o formulário.

As outras: o link "Materiais" está dentro do menu sanduíche no celular, e o
teste precisa abri-lo antes de clicar (2 falhas); e
`grantPermissions(['clipboard-write'])` não existe no WebKit (1 falha).

A última (1 falha, só no iPhone) pode ser defeito de verdade: o nome do evento
está no DOM da lista do painel mas nunca fica visível na largura do iPhone.

**Enquanto essa PR não entrar, as três histórias não estão em produção.** Hoje,
no site publicado, evento cancelado aparece na listagem pública igual a
qualquer outro, com data e local, sem nenhum aviso — alguém pode viajar para um
evento que não vai acontecer. Não há página de materiais e não há links fixados
no painel.

Nenhuma suíte reclamou disso, e as duas estavam certas: a do back-end testa o
back-end, e a do front intercepta chamadas que o front não faz. Interceptar só
verifica chamada que alguém escreveu — **tela que nunca pede um dado não tem o
que interceptar**, então a ausência da funcionalidade inteira passa
despercebida.

### Front-end da Sprint 4 — RF51, US13, US16 e US08

- **Responsáveis:** [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206) e [Renan Vieira Guedes](https://github.com/R-enanVieira)
- **Situação:** concluído e integrado à `main` ([PR #32](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/32)).

Quatro histórias escolhidas por um critério só: **o front entrega sozinho**. O
levantamento conferiu o código dos dois repositórios, e não o texto do backlog
— foi assim que a US02 ficou de fora da lista inicial, por depender de coluna
que não existe.

**RF51 — histórico de participações por convidado.** O dado já era calculado
na tela de convidados, para avisar antes de excluir alguém vinculado, mas só a
contagem aparecia e a lista era descartada. A agregação passou para o serviço
(`fetchParticipacoesPorConvidado`) e devolve os eventos inteiros, do mais
recente para o mais antigo. A listagem consome a mesma função, então contagem e
histórico não podem divergir.

**US13 (RF09) — duplicar evento.** A regra mora no serviço e é seletiva de
propósito: copia o que descreve o evento e **recusa** o que pertence àquela
edição. Datas não vêm (herdar a antiga nasceria um evento no passado), nem a
situação (a cópia nasce ativa), nem o álbum (o link aponta para as fotos da
edição anterior), nem os formulários de inscrição (misturaria as duas listas de
inscritos). Os convidados, esses sim, vêm marcados. O rascunho (RF08) ficou de
fora: exige campo novo no back-end.

**US16 (RNF02) — acessibilidade e leitor de tela.** Até aqui os rótulos
acessíveis entravam de carona, onde outra história passava. Duas lacunas
estruturais foram fechadas: 32 `<label>` em seis formulários não estavam
ligados ao campo que descreviam — o leitor de tela anunciava "caixa de edição"
sem dizer de quê — e o `Modal` era um `<div>` comum, que não se anunciava como
diálogo, não prendia o foco e não o devolvia ao fechar. Um dos testes é uma
varredura que reprova qualquer campo sem nome acessível em cinco formulários:
rótulo novo sem associação passa a quebrar o build.

**US08 (RNF03) — card de compartilhamento por evento.** As tags Open Graph
existiam, mas eram fixas no `index.html` e descreviam o site inteiro; o link
que as pessoas compartilham é o de um evento. Não dava para resolver no
cliente, porque o robô do WhatsApp não executa JavaScript. O `vercel.json`
passou a desviar `/eventos/:slug` para uma função na borda **só quando o
user-agent é de um robô de prévia**; visitante de verdade continua recebendo a
aplicação. A imagem sai pelo proxy do back-end, e não pelo link direto do
Drive, que responde 429/403 a quem vem de fora — era justamente a foto faltando
que a cliente relatou.

> Esta história estava atribuída a [Rivadalvio Joaquim da Silva
> Filho](https://github.com/RivaFilho) na matriz de rastreabilidade, que
> entregou as tags fixas. O que entrou agora é a parte por evento. Fica o
> registro para evitar trabalho duplicado.

### Três defeitos que estavam em produção

Levantamento pedido antes de publicar as histórias das Sprints 3 e 4. Os três
foram encontrados no código já publicado, e os três eram visíveis para quem usa
o sistema.

| Defeito | Onde | Correção |
|---|---|---|
| Cinco campos do evento eram digitados e **descartados em silêncio** | US02 | [PR #33](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/33) |
| Opções de status **inalcançáveis** no celular | US09 | [PR #34](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/34) |
| A página inicial anunciava como "próximo" um evento **já encerrado** | Home | [PR #35](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/35) |

**US02 — o dado sumia sem aviso.** O formulário de evento coleta vagas, contato
do responsável, valor, link de pagamento e regulamento desde a US02. Mas
`toApiEvent` não enviava nenhum dos cinco e `adaptEvent` não lia nenhum: a
administradora digitava, salvava, e o dado desaparecia — os campos nem são
obrigatórios, então nada sinalizava a perda. Os cinco passaram a ser mapeados
nos dois sentidos. **Continua dependendo do back-end:** as colunas não existem,
e enquanto não existirem a API ignora o campo desconhecido, de modo que o dado
ainda não persiste. O que deixou de acontecer é o front jogar fora em silêncio
o que foi digitado. Os nomes usados são assumidos, não um contrato publicado, e
ficaram isolados num lugar só com a nota de conferi-los antes de dar a
integração por concluída — na US09 três nomes assumidos estavam errados.

**US09 — trocar o status era impossível pelo telefone.** O `Dropdown`
posicionava o menu sempre logo abaixo do botão, sem conferir o espaço que
sobrava. Na última linha da tabela de voluntários, em tela de celular, o menu
caía fora da área visível e não havia mais página para rolar. Já era defeito
conhecido, registrado pela QA com `test.fail` na suíte de tela pequena. Agora o
menu abre para cima quando não cabe embaixo, o lado é limitado para não passar
da borda direita, e a anotação de defeito conhecido saiu do teste.

**Home — "próximo evento" apontando para o passado.** A escolha do destaque era
`proximos[0] || all[0]`: sem nenhum evento futuro, caía no primeiro da ordem da
API e a página inicial exibia "PRÓXIMO EVENTO" com contagem zerada e um botão
levando a um evento que já tinha acontecido. Além disso, o filtro olhava só a
data de início, então um evento de vários dias **já em curso** não contava como
próximo — a home anunciava outro mais distante justamente enquanto o evento
acontecia. O destaque passou a ser o primeiro evento que ainda não terminou, e
sem nada em cartaz não há destaque.

### Correções pedidas na revisão da PR #30

A revisão separou 28 testes vermelhos em quatro causas técnicas e uma quinta
que não foi possível descartar. **A quinta era defeito de produto**, e a
suspeita estava certa: na largura do iPhone sobram cerca de 88px para a coluna
do título na lista do painel, e o selo "Cancelado" ocupa 82px com `shrink-0`.
O título era espremido até largura zero — marcar um evento como cancelado ou
adiado fazia **o nome dele desaparecer** da lista no celular. Corrigido
deixando a linha quebrar.

Das outras quatro: a proposta de parametrizar o `setupApiMock` foi adotada, por
reaproveitar a semente do mock em vez de duplicar um evento dentro do teste; a
tag `@fonte` ficou só no teste que chama o serviço, e não no bloco inteiro,
para os demais continuarem rodando contra o build; o teste do celular passou a
abrir o menu sanduíche antes de clicar em "Materiais"; e a leitura da área de
transferência ficou condicionada ao navegador, porque o WebKit não conhece
essas permissões.

> Registro de um erro do trio: a verificação local havia rodado apenas os
> projetos `build` e `fonte`. Os projetos de celular não foram executados, e
> foi exatamente neles que os defeitos apareceram. Passou a ser regra rodar os
> quatro antes de abrir PR.

### Achados sobre a própria documentação

- **Responsável:** [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin)
- **Situação:** registrados.

**O `mkdocs.yml` da `main` aponta para duas páginas que não existem nela:**
`projeto/guia_front_us10_us11_us14.md` e `projeto/front_us10_us11_us14.md`. São
quatro links quebrados no site publicado — o `sprint03.md` e o
`backlog_melhorias.md` também apontam para elas. As duas páginas existem na
branch `docs/restaura-paginas-us10-us11-us14`, que resolve o problema assim que
for integrada.

**O resumo da Sprint 3 conta US10, US11 e US14 como completas nas duas
pontas.** Não estão: o front-end das três está na
[PR #30](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/30), ainda
aberta. O `backlog_melhorias.md` está correto — diz "falta o front-end". A
divergência fica registrada aqui, sem reescrever a Sprint 3, que já foi
fechada e apresentada.

---

## Resumo

- *Entregas de QA:* 3 — QA32 a QA34, por [João Pedro](https://github.com/Jadequilin)
- *Testes com Selenium:* 53 no total — 39 funcionais sobre a pilha real ([PR #31](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/31)) e 14 de entrega do build e de produção (QA32)
- *Histórias cobertas por Selenium:* 13 — US01, US02, US03, US04, US05, US07, US11, US14, US18, US19, US20, US22 e US23. Seguem sem cobertura por Selenium a US06, US08, US09, US10 e US15
- *Testes exploratórios:* 2 sessões, 6 achados — 2 corrigidos nesta sprint, 1 travado como teste verde, 3 como especificação executável, e 1 à espera da PR #30
- *Suíte do back-end:* 928 testes, 98,77% de cobertura (piso de 97%), pylint 9,88 (piso de 9,5)
- *Entregas de front-end:* 4 histórias (RF51, US13, US16 e US08) e 3 correções de defeitos que estavam em produção (US02, US09 e Home), por [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206) e [Renan Vieira Guedes](https://github.com/R-enanVieira)
- *Líder da Apresentação:* a definir
- *Início da Sprint:* 30/10/2026
- *Fim da Sprint:* 19/11/2026

---

## Em aberto ao fim da Sprint 4

A Sprint 5 fica reservada ao que a cliente apontar, então o que segue abaixo
não está estacionado nela: ou entra ainda nesta sprint, ou fica registrado como
dívida conhecida da entrega.

- **Rotacionar o *refresh token* do Google.** A correção do código conserta o
  vazamento futuro, não o passado: o token que está em produção hoje já passou
  por log e deve ser considerado comprometido.
- ~~**Integrar a [PR #30](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/30)**~~
  — **concluído.** As cinco causas da revisão foram tratadas, o CI fechou verde
  e a PR foi integrada à `main`, junto da Sprint 4 e das três correções de
  produção. Falta a publicação: a `main` do fork precisa chegar ao repositório
  `idbjovemnacional`.
- **As três correções propostas na EX-02**, uma validação cada, que dependem de
  alguém rodar o fluxo integrado antes de integrar.
- **Tirar a rota `/test-coverage` do build de produção**, sem quebrar a suíte
  Playwright que depende dela.
- **Selenium nas cinco histórias descobertas** (US06, US08, US09, US10 e US15).
- **Firefox no CI**, depois de uma execução manual limpa.
- **Colunas da US02 no back-end** (`vagas`, `contato_responsavel`,
  `valor_investimento`, `link_pagamento`, `link_regulamento`). O front já envia
  e lê os cinco; sem as colunas, a API ignora o campo e o dado não persiste. Os
  nomes foram assumidos pelo front e precisam ser conferidos antes de dar a
  integração por concluída.
- ~~**Integrar `docs/restaura-paginas-us10-us11-us14`**~~ — **concluído**
  ([PR #19](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Documentacao/pull/19)).
  As duas páginas voltaram pelo conteúdo exato que estava versionado, e a
  navegação do MkDocs resolve de novo.

---

## Histórico de Versão

| Versão | Data | Descrição | Autor(es) |
|---|---|---|---|
| `1.0` | 15/11/2026 | Criação do relatório da Sprint 04 com as entregas de QA (QA32 a QA34): testes com Selenium contra a entrega do build e contra a produção, as três amarrações no pipeline, duas sessões de testes exploratórios com seis achados, a correção dos dois defeitos de severidade alta, a revisão da PR #30 e os achados sobre a própria documentação | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) |
| `1.1` | 16/11/2026 | Registro da entrega de front-end da Sprint 4 — RF51, US13, US16 e US08 —, das três correções de defeitos que estavam em produção (US02, US09 e Home) e do atendimento à revisão da PR #30, incluindo o defeito de produto que ela apontou | [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206) e [Renan Vieira Guedes](https://github.com/R-enanVieira) |
