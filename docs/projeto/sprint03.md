# Sprint 03

*Período:* 07/10/2026 a 27/10/2026

> Sprint em andamento. Este relatório registra, por enquanto, as entregas de
> garantia de qualidade do back-end e do pipeline. As entregas dos trios de
> desenvolvimento serão acrescentadas à medida que forem concluídas.

## Entregas da Sprint

| Trio de Desenvolvedores | Qtd. de US | US | Assunto das US |
|---|---:|---|---|
| [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) | 9 | US01, US02, US07, US09, US10, US11, US14, US19, US23 | Garantia de qualidade: portões de CI, correção da listagem pública de eventos, contrato das respostas da API, cobertura das integrações com o Google e especificação executável das melhorias validadas com a cliente |

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

**Resultados.** 64 testes novos no back-end e 1 defeito de severidade alta
corrigido. A suíte foi de 698 para 762 testes, com 15 `xfailed` — 2 defeitos
abertos da Sprint 2 e 13 regras ainda sem implementação, registradas de
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

Os 13 testes novos exercitam a rota contra o Postgres de verdade, no job que já
sobe banco e aplicação em container: variações de grafia, curingas (`%%`, `__`,
aspas, tentativa de injeção) tratados como texto e o contrato de tamanho mínimo
do termo. Ficam marcados com `endpoint`, fora da suíte rápida.

### QA23 — Especificação executável das melhorias

As regras aceitas na reunião de 06/10/2026 entraram no
[backlog de melhorias](backlog_melhorias.md) e ainda não têm código. Em vez de
esperar a entrega dos trios para só então escrever teste, cada regra foi
registrada agora, de uma das duas formas:

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

Nenhum nome de campo foi combinado com os trios ainda, então cada regra aceita
qualquer um dos nomes plausíveis. Quem implementar com um nome fora da lista
acrescenta-o na própria PR — e a revisão fica sabendo que o contrato público da
API mudou.

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

### Ajuste em relação ao plano

O plano de QA previa começar a sprint pelos testes das histórias novas. Os
trios ainda não entregaram o código de US10, US11, US14, US19 e US23, e a
equipe optou por não ficar parada: as regras já validadas com a cliente viraram
especificação executável (QA23), e o esforço restante foi para o que não depende
de ninguém — os portões de CI, o defeito já registrado, o contrato das respostas
e as lacunas que a revisão por mutação da Sprint 2 havia exposto.

Isso também muda o que vai para a Sprint 5: ficam reservados para lá apenas os
itens que dependem de código que ainda não existe — os testes de ponta a ponta
de tela, que precisam de seletor para existir.

---

## Resumo

- *Histórias cobertas pela QA:* 9 — US01, US02, US07, US09, US10, US11, US14, US19 e US23
- *Total de US concluídas pelos trios:* a definir ao fim da sprint
- *Entregas de QA:* 7 (6 do back-end e pipeline, 1 do front-end)
- *Início da Sprint:* 07/10/2026
- *Fim da Sprint:* 27/10/2026

---

## Histórico de Versão

| Versão | Data | Descrição | Autor(es) |
|---|---|---|---|
| `1.0` | 19/10/2026 | Criação do relatório da Sprint 03 com as entregas de QA do back-end e do pipeline (QA18 a QA24), a correção do defeito da listagem pública e os achados da sprint | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) |
