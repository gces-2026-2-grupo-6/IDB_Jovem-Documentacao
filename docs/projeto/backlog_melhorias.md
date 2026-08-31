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

## Histórico de Versão

| Versão | Data | Descrição | Autor(es) |
|---|---|---|---|
| `1.0` | 31/08/2026 | Criação do backlog de melhorias a partir da elicitação com a cliente | [Júlia Massuda](https://github.com/JuliaReis18) |
