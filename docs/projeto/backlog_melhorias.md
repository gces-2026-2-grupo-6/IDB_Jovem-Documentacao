# Backlog de Melhorias

**Base:** formulário de elicitação aplicado em 25/08/2026 (tally.so/r/D46pyq)
**Respondente:** Raquel Gomes — Diretora Nacional de Adolescentes

## Sumário

- [Metodologia da Priorização](#metodologia)
- [Prioridade Alta](#alta)
- [Prioridade Média](#media)
- [Prioridade Baixa](#baixa)
- [Fora do Escopo desta Etapa](#fora)
- [Rastreabilidade](#rastreabilidade)
- [Requisitos Elicitados](#requisitos)

---

<a name="metodologia"></a>

## Metodologia da Priorização

O formulário coletou notas de 1 a 5 para 61 melhorias candidatas, distribuídas em nove grades temáticas. A análise das respostas revelou **forte concentração nas notas 4 e 5**: das linhas avaliadas, nenhuma recebeu nota inferior a 3, e a maioria absoluta ficou em 4. Isso é um comportamento esperado em escalas aplicadas a stakeholders que reconhecem valor em quase toda melhoria proposta.

Por esse motivo, a grade **não foi usada como critério único**. A priorização final combina três sinais, nesta ordem de peso:

1. **A pergunta aberta de desempate** — *"se só desse para entregar 3 melhorias neste semestre, quais seriam?"*. A resposta foi dada com mais reflexão que a grade e, de forma significativa, foi **repetida literalmente** na pergunta sobre critério de sucesso do projeto. Os três itens citados formam o núcleo da Prioridade Alta.
2. **Notas 5 isoladas dentro de cada grade** — quando uma linha se destaca em meio a 4s, a distinção é intencional.
3. **Dependência técnica e custo** — itens de custo trivial e alto impacto foram promovidos, conforme análise do código-fonte.

### Os três itens de desempate

> Cadastro de eventos · Galeria de diretores editável · Administrador por função

---

<a name="alta"></a>

## Prioridade Alta

<a name="us01"></a>

### US01 — Evento em múltiplos dias

<table>
<tbody>
<tr><td><strong>ID</strong></td><td>US01</td></tr>
<tr><td><strong>Rastreabilidade</strong></td><td>RF05, RF11</td></tr>
<tr><td><strong>Tema</strong></td><td>Gestão de Eventos</td></tr>
<tr><td><strong>Descrição</strong></td><td>Eu, como <em>administradora</em>, desejo <em>cadastrar um evento que ocorra em vários dias, inclusive em dias não consecutivos</em>, para <em>registrar acampamentos e congressos sem precisar criar um evento por dia</em>.</td></tr>
<tr><td><strong>Critérios de Aceitação</strong></td><td>- Formulário aceita data de início e data de término <br> - Permite marcar dias não consecutivos (ex.: três sábados seguidos) <br> - O evento aparece como <strong>um único card</strong> no calendário da página inicial, exibindo a faixa de datas <br> - O local do evento é único, não variando por dia</td></tr>
<tr><td><strong>Prioridade</strong></td><td>Alta</td></tr>
<tr><td><strong>Status</strong></td><td>Validado — citado entre as 3 melhorias essenciais</td></tr>
</tbody>
</table>

> **Nota técnica.** A tabela `evento` já possui `data_inicio` e `data_fim`, e o formulário já coleta ambas. A cliente respondeu **"tanto faz"** para programação separada por dia e **"não"** para local diferente por dia — ambas as simplificações reduzem o escopo previsto originalmente.

<a name="us02"></a>

### US02 — Informações completas do evento

<table>
<tbody>
<tr><td><strong>ID</strong></td><td>US02</td></tr>
<tr><td><strong>Rastreabilidade</strong></td><td>RF11</td></tr>
<tr><td><strong>Tema</strong></td><td>Gestão de Eventos</td></tr>
<tr><td><strong>Descrição</strong></td><td>Eu, como <em>administradora</em>, desejo <em>registrar informações completas do evento</em>, para <em>que o participante encontre tudo no site sem precisar perguntar no WhatsApp</em>.</td></tr>
<tr><td><strong>Critérios de Aceitação</strong></td><td>- Campos novos: valor/investimento, vagas limitadas, link de pagamento, contato do responsável e regulamento/anexo <br> - Campos opcionais não bloqueiam a criação do evento <br> - As informações aparecem na página pública do evento</td></tr>
<tr><td><strong>Prioridade</strong></td><td>Alta</td></tr>
<tr><td><strong>Status</strong></td><td>Validado — todos os cinco campos marcados</td></tr>
</tbody>
</table>

<a name="us03"></a>

### US03 — Administrador por setor

<table>
<tbody>
<tr><td><strong>ID</strong></td><td>US03</td></tr>
<tr><td><strong>Rastreabilidade</strong></td><td>RF01</td></tr>
<tr><td><strong>Tema</strong></td><td>Governança e Operações</td></tr>
<tr><td><strong>Descrição</strong></td><td>Eu, como <em>superadministradora</em>, desejo <em>conceder acesso a um administrador apenas do setor pelo qual ele responde</em>, para <em>que cada região administre o que lhe compete sem risco de alterar o restante do site</em>.</td></tr>
<tr><td><strong>Critérios de Aceitação</strong></td><td>- Setores com administrador próprio: <strong>Loja/Produtos</strong>, <strong>Agenda/Eventos</strong> e <strong>Inscrições</strong> <br> - Uma mesma pessoa pode responder por mais de um setor <br> - A exclusão de conteúdo é restrita ao superadministrador <br> - Apenas os superadministradores criam e removem administradores <br> - Não há separação de acesso entre Jovem e Teen <br> - O sistema comporta ao menos 6 administradores, um por região</td></tr>
<tr><td><strong>Prioridade</strong></td><td>Alta</td></tr>
<tr><td><strong>Status</strong></td><td>Validado — citado entre as 3 melhorias essenciais</td></tr>
</tbody>
</table>

> **Redução de escopo confirmada.** A cliente marcou apenas três setores. Galeria, Líderes e Voluntários **não** precisam de administrador próprio, o que diminui a matriz de permissões prevista originalmente.

<a name="us04"></a>

### US04 — Tradução automática indevida

<table>
<tbody>
<tr><td><strong>ID</strong></td><td>US04</td></tr>
<tr><td><strong>Rastreabilidade</strong></td><td>RF06, RF04</td></tr>
<tr><td><strong>Tema</strong></td><td>Experiência do Usuário e Engajamento</td></tr>
<tr><td><strong>Descrição</strong></td><td>Eu, como <em>visitante em dispositivo móvel</em>, desejo <em>que a página não seja traduzida automaticamente pelo navegador</em>, para <em>que os textos, os nomes próprios e as fotos permaneçam corretos</em>.</td></tr>
<tr><td><strong>Critérios de Aceitação</strong></td><td>- O documento declara idioma português brasileiro <br> - O navegador móvel não oferece tradução automática ao abrir a página <br> - Os nomes próprios permanecem inalterados <br> - <strong>A foto do Pr. Áquila deixa de aparecer duplicada na página inicial</strong></td></tr>
<tr><td><strong>Prioridade</strong></td><td>Alta</td></tr>
<tr><td><strong>Status</strong></td><td>Validado — a cliente relatou que ocorre <strong>sempre</strong></td></tr>
</tbody>
</table>

> **Causa raiz confirmada pela cliente.** Perguntada sobre onde viu a duplicação, respondeu: *"Na página inicial, quando faz tradução automática, eu recebi prints"*. A duplicação da foto é **consequência** da tradução, não defeito independente. Custo estimado: correção do atributo `lang` no `index.html`.

<a name="us05"></a>

### US05 — CRUD de líderes e diretores anteriores

<table>
<tbody>
<tr><td><strong>ID</strong></td><td>US05</td></tr>
<tr><td><strong>Rastreabilidade</strong></td><td>RF02, RF03</td></tr>
<tr><td><strong>Tema</strong></td><td>Governança e Operações</td></tr>
<tr><td><strong>Descrição</strong></td><td>Eu, como <em>superadministradora</em>, desejo <em>cadastrar e editar os líderes e a galeria de diretores anteriores pelo painel</em>, para <em>manter a página atualizada a cada troca de gestão sem depender da equipe técnica</em>.</td></tr>
<tr><td><strong>Critérios de Aceitação</strong></td><td>- Tela de CRUD no painel administrativo, <strong>restrita ao superadministrador</strong> <br> - Campos editáveis: nome, cargo, foto, região, mini-biografia e redes sociais <br> - A marcação de "diretor anterior" é <strong>manual</strong>, não automática <br> - A galeria de anteriores contempla <strong>apenas o cargo nacional</strong> <br> - Os líderes atuais deixam de estar fixos no código-fonte</td></tr>
<tr><td><strong>Prioridade</strong></td><td>Alta</td></tr>
<tr><td><strong>Status</strong></td><td>Validado — citado entre as 3 melhorias essenciais</td></tr>
</tbody>
</table>

> **Duas restrições de escopo vindas da resposta.** A cliente respondeu que prefere marcar o diretor anterior manualmente, e acrescentou em campo aberto: *"A galeria de diretores anteriores refere-se apenas ao cargo nacional"*. Ambas reduzem o esforço. Além disso, o back-end **já expõe o CRUD completo** de líderes — resta apenas a tela.

<a name="us06"></a>

### US06 — CRUD de bandas e palestrantes

<table>
<tbody>
<tr><td><strong>ID</strong></td><td>US06</td></tr>
<tr><td><strong>Rastreabilidade</strong></td><td>RF47, RF48, RF49</td></tr>
<tr><td><strong>Tema</strong></td><td>Gestão de Eventos</td></tr>
<tr><td><strong>Descrição</strong></td><td>Eu, como <em>administradora</em>, desejo <em>gerenciar bandas e palestrantes pelo painel e reaproveitá-los em novos eventos</em>, para <em>não recadastrar os mesmos convidados a cada edição</em>.</td></tr>
<tr><td><strong>Critérios de Aceitação</strong></td><td>- Tela de CRUD de bandas e palestrantes no painel <br> - Campos: nome, foto, função (banda, pregador, convidado), mini-biografia e redes sociais <br> - Os convidados aparecem na página pública do evento <br> - Um convidado já cadastrado pode ser vinculado a um novo evento sem recadastro <br> - Possível vincular o convidado a uma atividade específica da programação</td></tr>
<tr><td><strong>Prioridade</strong></td><td>Alta</td></tr>
<tr><td><strong>Status</strong></td><td>Validado — cinco das seis linhas da grade receberam nota 5</td></tr>
</tbody>
</table>

> **Área descoberta na auditoria de código.** Não constava de nenhum relato inicial da cliente. As tabelas `banda_palestrante` e `participa` já existem, assim como o `speakerService` no front-end. A cliente confirmou que usa o recurso **normalmente** e que os mesmos convidados participam de vários eventos **com frequência**.

---

<a name="media"></a>

## Prioridade Média

<a name="us07"></a>

### US07 — Álbuns de fotos por evento

<table>
<tbody>
<tr><td><strong>ID</strong></td><td>US07</td></tr>
<tr><td><strong>Rastreabilidade</strong></td><td>RF07, RF23</td></tr>
<tr><td><strong>Tema</strong></td><td>Experiência do Usuário e Engajamento</td></tr>
<tr><td><strong>Descrição</strong></td><td>Eu, como <em>administradora</em>, desejo <em>que as fotos fiquem organizadas em álbuns separados por evento</em>, para <em>que as imagens de eventos diferentes não se misturem</em>.</td></tr>
<tr><td><strong>Critérios de Aceitação</strong></td><td>- Cada evento possui seu próprio álbum de fotos <br> - As fotos de um evento não aparecem misturadas às de outro <br> - O processo de inclusão de fotos é simplificado em relação ao atual <br> - Possível definir foto de capa e reordenar as imagens</td></tr>
<tr><td><strong>Prioridade</strong></td><td>Média</td></tr>
<tr><td><strong>Status</strong></td><td>Validado — nota 5 isolada na grade da galeria</td></tr>
</tbody>
</table>

> **O problema não era o que a equipe supôs.** A hipótese inicial apontava fragilidade dos links do Google Drive. A cliente esclareceu: *"É muito complexo a forma de incluir fotos na galeria, pois os eventos se misturam"*. O problema é de **organização**, não de infraestrutura de imagem.

<a name="us08"></a>

### US08 — Metadados de compartilhamento (Open Graph)

<table>
<tbody>
<tr><td><strong>ID</strong></td><td>US08</td></tr>
<tr><td><strong>Rastreabilidade</strong></td><td>RNF03</td></tr>
<tr><td><strong>Tema</strong></td><td>Experiência do Usuário e Engajamento</td></tr>
<tr><td><strong>Descrição</strong></td><td>Eu, como <em>divulgadora</em>, desejo <em>que o link do site exiba foto e título ao ser compartilhado</em>, para <em>que a divulgação no WhatsApp e no Instagram tenha aparência profissional</em>.</td></tr>
<tr><td><strong>Critérios de Aceitação</strong></td><td>- Metadados Open Graph configurados <br> - Prévia com imagem, título e descrição ao compartilhar <br> - Verificado em WhatsApp e Instagram</td></tr>
<tr><td><strong>Prioridade</strong></td><td>Média</td></tr>
<tr><td><strong>Status</strong></td><td>Validado — defeito relatado espontaneamente</td></tr>
</tbody>
</table>

> A cliente escreveu em campo aberto: *"Ao compartilhar aparece o nome do link e não aparece foto."* Custo trivial, alto impacto percebido.

<a name="us09"></a>

### US09 — Inscrição separada para voluntários

<table>
<tbody>
<tr><td><strong>ID</strong></td><td>US09</td></tr>
<tr><td><strong>Rastreabilidade</strong></td><td>RF52 <em>(novo)</em></td></tr>
<tr><td><strong>Tema</strong></td><td>Gestão de Eventos</td></tr>
<tr><td><strong>Descrição</strong></td><td>Eu, como <em>administradora</em>, desejo <em>disponibilizar dois links de inscrição por evento — um para participantes e outro para voluntários</em>, para <em>separar quem vai participar de quem vai trabalhar no evento</em>.</td></tr>
<tr><td><strong>Critérios de Aceitação</strong></td><td>- O evento comporta dois fluxos de inscrição distintos <br> - Cada fluxo tem seu próprio link e sua própria listagem <br> - O fluxo de voluntariado mantém o status pendente/aprovado/reprovado atual</td></tr>
<tr><td><strong>Prioridade</strong></td><td>Média</td></tr>
<tr><td><strong>Status</strong></td><td>Requisito novo — surgido de campo aberto</td></tr>
</tbody>
</table>

> **Requisito não previsto no catálogo.** A cliente escreveu: *"Nessa etapa podia ter a inscrição do evento e também de voluntários, seriam 2 links então."* Hoje o sistema contempla **apenas** o fluxo de voluntariado.

<a name="us10"></a>

### US10 — Cadastro nacional de líderes

<table>
<tbody>
<tr><td><strong>ID</strong></td><td>US10</td></tr>
<tr><td><strong>Rastreabilidade</strong></td><td>RF53 <em>(novo)</em></td></tr>
<tr><td><strong>Tema</strong></td><td>Governança e Operações</td></tr>
<tr><td><strong>Descrição</strong></td><td>Eu, como <em>diretora nacional</em>, desejo <em>manter um cadastro nacional de líderes de jovens e adolescentes no site</em>, para <em>deixar de controlar essa informação em planilhas e mensagens</em>.</td></tr>
<tr><td><strong>Critérios de Aceitação</strong></td><td>- Cadastro de líderes por região <br> - Distinto da galeria institucional da página inicial <br> - Consulta e edição restritas a perfis autorizados <br> - Escopo a detalhar com a cliente antes da implementação</td></tr>
<tr><td><strong>Prioridade</strong></td><td>Média</td></tr>
<tr><td><strong>Status</strong></td><td>Requisito novo — necessita refinamento</td></tr>
</tbody>
</table>

> Não confundir com a galeria de diretores da US05 — este é um cadastro operacional de porte maior. Recomenda-se refinamento com a cliente antes de estimar.

<a name="us11"></a>

### US11 — Materiais e conteúdo institucional

<table>
<tbody>
<tr><td><strong>ID</strong></td><td>US11</td></tr>
<tr><td><strong>Rastreabilidade</strong></td><td>RF54 <em>(novo)</em>, RF25, RF26</td></tr>
<tr><td><strong>Tema</strong></td><td>Experiência do Usuário e Engajamento</td></tr>
<tr><td><strong>Descrição</strong></td><td>Eu, como <em>jovem ou adolescente</em>, desejo <em>encontrar no site os materiais para download, a missão, a visão e os valores</em>, para <em>acessar o conteúdo da organização sem pedir a alguém</em>.</td></tr>
<tr><td><strong>Critérios de Aceitação</strong></td><td>- Seção de materiais disponíveis para download <br> - Conteúdo institucional (missão, visão e valores) publicado e editável pelo painel <br> - Navegação clara a partir da página inicial</td></tr>
<tr><td><strong>Prioridade</strong></td><td>Média</td></tr>
<tr><td><strong>Status</strong></td><td>Requisito novo — origem em reclamação recorrente</td></tr>
</tbody>
</table>

> Resposta sobre reclamações recorrentes dos jovens: *"Eventos, missão, visão e valores e material de download."* A área de materiais para download **não existe** no sistema atual.

---

<a name="baixa"></a>

## Prioridade Baixa

| ID | História | Rastreabilidade | Justificativa da posição |
|---|---|---|---|
| US12 | Cadastro de produtos com preço, tamanho e disponibilidade | RF32, RF33 | A cliente respondeu *"queremos usar, mas ainda não cadastramos os produtos"* — a loja **permanece no escopo**, mas a venda segue por link externo |
| US13 | Rascunho e duplicação de evento | RF08, RF09 | Nota 4 na grade; não citado no desempate |
| US14 | Marcar evento como cancelado ou adiado | RF12 | Nota 4; baixo custo, candidato a entrada oportunista |
| US15 | Histórico de eventos passados com fotos | RF13 | Nota 4; depende de US07 |
| US16 | Acessibilidade e leitor de tela | RNF02 | Nota 4; sem demanda concreta relatada |
| US17 | Controle de direito de imagem de menores | RF07 | A cliente marcou *"sim, precisa de controle"* — escopo a definir |

---

<a name="fora"></a>

## Fora do Escopo desta Etapa

Itens conscientemente descartados, registrados para rastreabilidade da decisão.

| Item | Rastreabilidade | Motivo |
|---|---|---|
| Otimização de desempenho no celular | RNF01 | A cliente classificou o site como **"muito rápido"** e informou que nenhuma página específica demora |
| Agenda gerenciada sem depender do Google Calendar | RF46 | A cliente respondeu **"não"**. O risco de continuidade é mitigado: *"O e-mail é institucional, então passamos a senha"* |
| Separação de acesso entre Jovem e Teen | RF01 (parcial) | Respondido **"não"** |
| Local diferente por dia do evento | RF18 | Respondido **"não"** |
| Programação obrigatoriamente separada por dia | RF05 (parcial) | Respondido **"tanto faz"** — implementar apenas se vier de graça com a US01 |
| Notificação automática ao voluntário | RF36 | A cliente considera o retorno manual por e-mail adequado: *"Assim está ótimo"* |
| Mudança automática de diretor para "anterior" | RF03 (parcial) | Respondido **"não, prefiro marcar manualmente"** |
| Versão do site em outro idioma | RF25 (parcial) | Respondido **"talvez no futuro"** — única nota 3 da grade de textos |

> **Ressalva sobre a senha institucional.** Embora a cliente considere o risco resolvido, compartilhar a senha de uma conta institucional entre pessoas é prática insegura. Recomenda-se registrar a observação no relatório técnico, ainda que o requisito não seja implementado nesta etapa.

---

<a name="rastreabilidade"></a>

## Rastreabilidade

| História | Requisitos | Origem da priorização |
|---|---|---|
| US01, US02 | RF05, RF11 | Desempate — *"cadastro de eventos"* |
| US03 | RF01 | Desempate — *"administrador por função"* |
| US04 | RF06, RF04 | Desempate indireto + defeito confirmado |
| US05 | RF02, RF03 | Desempate — *"galeria de diretores editável"* |
| US06 | RF47 a RF51 | Cinco notas 5 na grade |
| US07 | RF07, RF23 | Nota 5 isolada + campo aberto |
| US08 | RNF03 | Defeito relatado espontaneamente |
| US09 | RF52 | Campo aberto — requisito novo |
| US10 | RF53 | Campo aberto — requisito novo |
| US11 | RF54, RF25, RF26 | Campo aberto + grade nota 5 |

### Referências indicadas pela cliente

Sites citados como inspiração, úteis para a etapa de prototipação:

- itsbr.com.br
- jesuscopy.com
- dunamismovement.com

---

<a name="requisitos"></a>

## Requisitos Elicitados

Catálogo completo dos requisitos levantados na elicitação, usado como referência para a coluna **Rastreabilidade** das histórias acima. São **64 requisitos**: 7 originados de relatos diretos da cliente, 54 propostos pela equipe a partir da análise do código-fonte e 3 descobertos nos campos abertos do formulário.

Cada requisito possui campo de **Responsável** e de **O que foi feito**. O primeiro identifica quem assumiu o item; o segundo descreve a entrega de forma que possa ser verificada sem abrir o código.

**Legenda de origem:** `C` relato direto da cliente · `C+T` relato confirmado por evidência técnica · `T` achado da equipe na análise do código · `S` sugestão da equipe · `A` resposta aberta do formulário

### Relatos originais da cliente

| ID | Requisito | Origem | Situação | Responsável | O que foi feito |
|---|---|---|---|---|---|
| RF01 | Administradores com permissão separada por setor | C+T | US03 — Alta | QA: [@Jadequilin](https://github.com/Jadequilin)<br>Implementação: [@daviRolvr](https://github.com/daviRolvr), [@JoaoPedro2206](https://github.com/JoaoPedro2206), [@R-enanVieira](https://github.com/R-enanVieira) | **QA:** 44 testes de autorização por papel: matriz de 13 rotas, negação de acesso e escalonamento de privilégio. Duas vulnerabilidades documentadas na [Matriz de Autorização](matriz-autorizacao.md), uma delas corrigida.<br>**Implementação:** três setores lidos dos papéis do Keycloak, com menu, painel e rotas filtrados por setor; exclusão de conteúdo restrita à superadministradora, recusada também na camada de serviço; tela de gestão de administradores. |
| RF02 | Cadastrar e editar líderes atuais pelo painel | C+T | US05 — Alta | Implementação: [@BrzGab](https://github.com/BrzGab), [@SamaraAlvess](https://github.com/SamaraAlvess), [@Joaovitor045](https://github.com/Joaovitor045) | **Implementação:** tela "Diretores & Líderes" no painel, restrita à superadministradora — o administrador comum não vê o item no menu e é redirecionado ao acessar a rota. Listagem em abas (atuais e anteriores), cadastro, edição e exclusão com confirmação. Campos: nome, cargo, foto (link do Drive com pré-visualização), região, redes sociais, mini-biografia, ordem de exibição e marcação manual de "diretor anterior". A seção "Nosso Organograma" da página inicial passou a ler os líderes da API: deixaram de estar fixos no código-fonte. 12 testes E2E. [PR #2](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/2).<br>**Pendência no back-end:** persistir `regiao`, `bio`, `redes_sociais` e `gestao` — o front já envia os quatro campos, que hoje são ignorados pelo schema. |
| RF03 | Galeria de diretores anteriores editável | C | US05 — Alta (restrita ao cargo nacional) | Implementação: [@BrzGab](https://github.com/BrzGab), [@SamaraAlvess](https://github.com/SamaraAlvess), [@Joaovitor045](https://github.com/Joaovitor045) | **Implementação:** aba "Diretores anteriores" no painel e "Galeria de Diretores" na página inicial listam apenas quem foi marcado manualmente como anterior **e** tem cargo nacional. O formulário avisa quando um líder regional marcado como anterior não entrará na galeria e coleta o período de gestão exibido no card ("Gestão 2020 – 2023"). [PR #2](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/2). |
| RF04 | Corrigir foto duplicada do Pr. Áquila | C+T | US04 — Alta (causa: tradução automática) | QA e implementação: [@RivaFilho](https://github.com/RivaFilho)<br>Implementação: [@BrzGab](https://github.com/BrzGab), [@SamaraAlvess](https://github.com/SamaraAlvess), [@Joaovitor045](https://github.com/Joaovitor045) | **QA:** 4 testes de ausência de líder duplicado — nas duas abas da seção, repetição de imagem e estabilidade da contagem ao alternar entre elas. Rodados antes da correção, os quatro já passavam: sem tradutor ativo as listas não duplicam, o que confirma que o defeito é artefato da tradução e não cadastro repetido.<br>**Implementação:** resolvido junto com RF06 — a duplicação some com a correção do idioma. No [PR #6](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/6), os nomes dos líderes (página inicial e painel) receberam `translate="no"`, e um teste de regressão percorre as duas abas da seção verificando que não há nome nem foto repetidos. |
| RF05 | Evento com múltiplos dias | C+T | US01 — Alta | QA: [@Jadequilin](https://github.com/Jadequilin)<br>Implementação: [@daviRolvr](https://github.com/daviRolvr), [@JoaoPedro2206](https://github.com/JoaoPedro2206), [@R-enanVieira](https://github.com/R-enanVieira) | **QA:** 16 testes de faixa de datas: até três dias, dias não consecutivos, virada de mês e de ano, rejeição de datas invertidas.<br>**Implementação:** card único no calendário com a faixa de datas; evento em andamento deixa de sumir do site; evento que atravessa a virada aparece nos dois meses; seleção de dias não consecutivos no formulário. |
| RF06 | Corrigir tradução automática indevida no celular | C+T | US04 — Alta | QA e implementação: [@RivaFilho](https://github.com/RivaFilho)<br>Implementação: [@BrzGab](https://github.com/BrzGab), [@SamaraAlvess](https://github.com/SamaraAlvess), [@Joaovitor045](https://github.com/Joaovitor045) | **QA:** 2 testes verificando que o documento declara `pt-BR`, na página inicial e em rotas internas. Ambos falhavam antes da correção. Suíte de `tests/public` e `tests/layout` reexecutada: 108 passando.<br>**Implementação:** `lang="en"` corrigido para `pt-BR` no `index.html`. O navegador deixa de oferecer tradução automática, e com isso para de reescrever os nós de texto e quebrar a reconciliação do React.<br>**Complemento ([PR #6](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/6)):** meta `notranslate`, para que o Chrome não ofereça tradução mesmo com o celular em outro idioma; `translate="no"` no título da marca; `lang` e nome do aplicativo no `site.webmanifest`; 4 testes de regressão — idioma do documento, meta, nomes não traduzíveis e ausência de duplicação. |
| RF07 | Corrigir problemas da galeria de fotos | C | US07 — Média | | |

### Gestão de eventos

| ID | Requisito | Origem | Situação | Responsável | O que foi feito |
|---|---|---|---|---|---|
| RF08 | Rascunho: criar evento e publicar depois | S | US13 — Baixa | | |
| RF09 | Duplicar evento anterior para nova edição | S | US13 — Baixa | | |
| RF10 | Eventos recorrentes automáticos | S | Backlog | | |
| RF11 | Campos novos: vagas, valor, faixa etária, prazo, o que levar | S | US02 — Alta | QA: [@Jadequilin](https://github.com/Jadequilin)<br>Implementação: [@daviRolvr](https://github.com/daviRolvr), [@JoaoPedro2206](https://github.com/JoaoPedro2206), [@R-enanVieira](https://github.com/R-enanVieira) | **QA:** testes de campos complementares: link de formulário, link de galeria e campos opcionais ausentes não bloqueiam a criação.<br>**Implementação (parcial, via US01):** reestruturação do formulário de evento e da validação de datas, que a US02 estende com os cinco campos novos. |
| RF12 | Marcar evento como cancelado ou adiado | S | US14 — Baixa | | |
| RF13 | Histórico de eventos passados com fotos | S | US15 — Baixa | | |
| RF14 | Inscrição de voluntários dentro do site | S | Backlog | | |
| RF15 | Confirmação de inscrição por e-mail | S | Backlog | | |
| RF16 | Controle de vagas com lista de espera | S | Backlog | | |
| RF17 | Exportar lista de inscritos em planilha | S | Backlog | | |
| RF18 | Local diferente por dia do evento | S | **Fora do escopo** | | |
| RF19 | Responsável indicado em cada atividade | S | Backlog | | |
| RF20 | Botão "adicionar à minha agenda" | S | Backlog | | |
| RF21 | Contagem regressiva do próximo evento | S | Backlog | | |

### Conteúdo e comunicação

| ID | Requisito | Origem | Situação | Responsável | O que foi feito |
|---|---|---|---|---|---|
| RF22 | Upload de imagens direto no site | T | Backlog | | |
| RF23 | Álbum de fotos separado por evento | T | US07 — Média | | |
| RF24 | Vídeos além de fotos na galeria | S | Backlog | | |
| RF25 | Editar textos institucionais pelo painel | S | US11 — Média | | |
| RF26 | Seção de avisos e comunicados na home | S | US11 — Média | | |
| RF27 | Espaço para devocional ou conteúdo semanal | S | Backlog | | |
| RF28 | Integração com Instagram | S | Backlog | | |
| RF29 | Página com horários de culto e como chegar | S | Backlog | | |
| RF30 | Formulário de contato com a liderança | S | Backlog | | |
| RF31 | Lista de e-mails para avisos | S | Backlog | | |

### Loja e produtos

| ID | Requisito | Origem | Situação | Responsável | O que foi feito |
|---|---|---|---|---|---|
| RF32 | Preço, tamanho e cor nos produtos | S | US12 — Baixa | | |
| RF33 | Categorias de produtos | S | US12 — Baixa | | |
| RF34 | Vincular produto a um evento | S | Backlog | | |
| RF35 | Reserva ou pedido com envio para o WhatsApp | S | Backlog | | |

### Voluntários

| ID | Requisito | Origem | Situação | Responsável | O que foi feito |
|---|---|---|---|---|---|
| RF36 | Notificar voluntário sobre aprovação ou reprovação | S | **Fora do escopo** | | |
| RF37 | Registrar área de atuação do voluntário | S | Backlog | | |
| RF38 | Exportar lista de voluntários em planilha | S | Backlog | | |
| RF39 | Check-in de voluntários no dia do evento | S | Backlog | | |
| RF40 | Histórico de participação por voluntário | S | Backlog | | |
| RF41 | Escala de voluntários por atividade | S | Backlog | | |

### Gestão e segurança

| ID | Requisito | Origem | Situação | Responsável | O que foi feito |
|---|---|---|---|---|---|
| RF42 | Log de auditoria de alterações | S | Backlog | | |
| RF43 | Acesso administrativo temporário | S | Backlog | | |
| RF44 | Recuperação de senha do administrador | S | Backlog | | |
| RF45 | Painel inicial com números | S | Backlog | | |
| RF46 | Agenda sem depender de conta Google pessoal | T | **Fora do escopo** | | |

### Bandas e palestrantes

*Área descoberta na auditoria de código, ausente dos relatos iniciais.*

| ID | Requisito | Origem | Situação | Responsável | O que foi feito |
|---|---|---|---|---|---|
| RF47 | Gerenciar bandas e palestrantes pelo painel | T | US06 — Alta | [@daviRolvr](https://github.com/daviRolvr), [@JoaoPedro2206](https://github.com/JoaoPedro2206), [@R-enanVieira](https://github.com/R-enanVieira) | Tela de convidados no painel, com busca por nome e função, cadastro, edição e exclusão. Construída sobre o CRUD que a API já oferecia e que nenhuma tela alcançava. A função (banda, pregador, convidado) é derivada do campo de texto livre da API, sem descaracterizar o que já estava cadastrado. |
| RF48 | Exibir convidados na página pública do evento | S | US06 — Alta | [@daviRolvr](https://github.com/daviRolvr), [@JoaoPedro2206](https://github.com/JoaoPedro2206), [@R-enanVieira](https://github.com/R-enanVieira) | Convidados separados por função na página do evento: antes o título era fixo em "Palestrantes" e uma banda aparecia anunciada como palestrante. Grupo sem ninguém não é exibido. |
| RF49 | Reaproveitar convidado em novo evento | S | US06 — Alta | [@daviRolvr](https://github.com/daviRolvr), [@JoaoPedro2206](https://github.com/JoaoPedro2206), [@R-enanVieira](https://github.com/R-enanVieira) | No formulário do evento, o texto livre deu lugar à seleção dos já cadastrados, com cadastro rápido para quem ainda não existe. O vínculo passou a ser por identificador: antes era resolvido comparando nomes, e corrigir a grafia de um nome criava um cadastro novo. |
| RF50 | Vincular convidado a atividade da programação | S | US06 — Alta | [@daviRolvr](https://github.com/daviRolvr), [@JoaoPedro2206](https://github.com/JoaoPedro2206), [@R-enanVieira](https://github.com/R-enanVieira) | **Não entregue.** Exige tabela nova no back-end ligando convidado a atividade — a `atividade` só conhece o evento. Levado a refinamento em vez de virar tela que não guarda nada. |
| RF51 | Histórico de participações por convidado | S | Backlog | [@daviRolvr](https://github.com/daviRolvr), [@JoaoPedro2206](https://github.com/JoaoPedro2206), [@R-enanVieira](https://github.com/R-enanVieira) | **Parcial.** A listagem de convidados mostra em quantos eventos cada um já esteve, e a confirmação de exclusão avisa quando o convidado está anunciado em algum. Não há tela de histórico por convidado — o dado existe, falta a visualização dedicada. |

### Requisitos surgidos das respostas abertas

| ID | Requisito | Origem | Situação | Responsável | O que foi feito |
|---|---|---|---|---|---|
| RF52 | Inscrição de participante, separada da de voluntário | A | US09 — Média | | |
| RF53 | Cadastro nacional de líderes de jovens e adolescentes | A | US10 — Média | | |
| RF54 | Área de materiais para download | A | US11 — Média | | |

### Requisitos não funcionais

| ID | Requisito | Origem | Situação | Responsável | O que foi feito |
|---|---|---|---|---|---|
| RNF01 | Otimização de desempenho no celular | S | **Fora do escopo** — site classificado como "muito rápido" | | |
| RNF02 | Acessibilidade e leitor de tela | S | US16 — Baixa | [@daviRolvr](https://github.com/daviRolvr), [@JoaoPedro2206](https://github.com/JoaoPedro2206), [@R-enanVieira](https://github.com/R-enanVieira) | **Parcial e incidental.** Rótulos acessíveis acrescentados onde as histórias passaram: navegação de mês do calendário, campos de dia do evento, busca de convidados e botões de remover. Não houve revisão de acessibilidade do site — isso segue sendo o escopo da US16. |
| RNF03 | Prévia correta ao compartilhar o link | S | US08 — Média | | |
| RNF04 | SEO — melhor posicionamento no Google | S | Backlog | | |
| RNF05 | Layout revisado em telas pequenas | S | Backlog | | |
| RNF06 | Mensagem de erro clara no lugar de tela branca | S | Backlog | | |
| RNF07 | Backup e restauração dos dados | S | Backlog | | |
| RNF08 | Padronização de fuso horário nas datas | T | Backlog | [@daviRolvr](https://github.com/daviRolvr), [@JoaoPedro2206](https://github.com/JoaoPedro2206), [@R-enanVieira](https://github.com/R-enanVieira) | **Parcial, no front.** A leitura de data passou a usar o relógio de parede da string em vez de `new Date`, que interpretava data sem hora como UTC e mostrava um dia a menos. Verificado contra o back-end: a coluna é `timestamptz`, não há conversão no caminho, e a ida e volta é estável. |
| RNF09 | Funcionar bem com internet ruim | S | Backlog | | |
| RNF10 | Manual de uso do painel para a equipe | S | Backlog | | |

---

## Histórico de Versão

| Versão | Data | Descrição | Autor(es) |
|---|---|---|---|
| `1.0` | 31/08/2026 | Criação do backlog de melhorias a partir da elicitação com a cliente | [Júlia Massuda](https://github.com/JuliaReis18) |
| `1.1` | 31/08/2026 | Inclusão do catálogo completo de requisitos elicitados, com coluna de responsável por requisito | [João Pedro](https://github.com/Jadequilin) |
| `1.2` | 11/09/2026 | Inclusão do campo "O que foi feito" no catálogo e registro das entregas de QA da Sprint 1 | [João Pedro](https://github.com/Jadequilin) |
| `1.3` | 13/09/2026 | Registro da implementação dos requisitos das histórias US01, US03 e US06 no catálogo, ao lado das entregas de QA já cadastradas | [Davi Emanuel](https://github.com/daviRolvr) |
| `1.4` | 13/09/2026 | Registro dos requisitos tocados de forma parcial pelas histórias US01, US03 e US06 — RF51, RNF02 e RNF08 | [Davi Emanuel](https://github.com/daviRolvr) |
| `1.5` | 13/09/2026 | Registro das entregas de QA do front-end — correção do idioma, metadados de compartilhamento e testes de regressão da duplicação | [João Pedro](https://github.com/Jadequilin) e [Rivadalvio](https://github.com/RivaFilho)   |
| `1.6` | 13/09/2026 | Registro da implementação das histórias US04 e US05 no catálogo — RF02, RF03, RF04 e RF06 | [Gabriel Lopes](https://github.com/BrzGab), [Samara Alves](https://github.com/SamaraAlvess) e [João Vitor](https://github.com/Joaovitor045) |