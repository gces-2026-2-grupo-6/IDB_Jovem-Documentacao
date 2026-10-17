# Backlog de Melhorias

**Base:** formulário de elicitação aplicado em 25/08/2026 (tally.so/r/D46pyq)
**Respondente:** Raquel Gomes — Diretora Nacional de Adolescentes
**Atualização:** reunião de validação com a cliente em 16/10/2026

## Sumário

- [Metodologia da Priorização](#metodologia)
- [Validação de 16/10/2026](#validacao)
- [Backlog Priorizado](#priorizado)
- [Prioridade Alta](#alta)
- [Prioridade Média](#media)
- [Prioridade Baixa](#baixa)
- [Fora do Escopo desta Etapa](#fora)
- [Rastreabilidade](#rastreabilidade)
- [Dúvidas para confirmar com a Pra. Raquel](#duvidas)
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

### Revisão de 16/10/2026

Na reunião de validação, a cliente percorreu um roteiro com as entregas das Sprints 1 e 2 e as perguntas em aberto, e anotou as respostas à mão. Desta reunião em diante, **a ordem definida por ela prevalece sobre os três sinais acima**. Os sinais seguem valendo para as histórias que ela não ordenou.

Regras adotadas na revisão:

- Onde a resposta foi ambígua ou ilegível, o critério aparece como **PENDENTE – confirmar com a cliente** e remete a uma pergunta da lista de [Dúvidas](#duvidas). Nenhum requisito foi deduzido além do que ela disse.
- As histórias novas e as revisadas seguem o formato *"Como &lt;papel&gt;, quero &lt;ação&gt;, para &lt;benefício&gt;"*, com critérios em **Dado / Quando / Então**, tamanho estimado e dependências.
- As falas da cliente aparecem entre aspas, conforme a transcrição das anotações.

**Tamanho (estimativa inicial, a revisar pelos trios no planejamento):** `P` cabe com folga numa sprint para um trio · `M` ocupa um trio durante a sprint · `G` precisa ser dividida ou envolve front-end e back-end por mais de uma sprint.

---

<a name="validacao"></a>

## Validação de 16/10/2026

Itens que a cliente aprovou na reunião.

| Item apresentado | História | Resposta | Situação |
|---|---|---|---|
| Tradução automática e foto duplicada | [US04](#us04) | "OK" | Validada. No ar desde 05/10/2026 |
| Página de líderes | [US05](#us05) | "OK" | Validada. No ar desde 05/10/2026 |
| Evento de vários dias | [US01](#us01) | "OK" | Validada |
| Galeria por álbum de evento | [US07](#us07) | "OK" | Validada. Aguarda publicação |
| Dois caminhos de inscrição (participante × voluntário) | [US09](#us09) | "OK", texto dos botões aprovado | Validada. Aguarda publicação |
| Histórico de eventos passados | [US15](#us15) | "Não é preocupação." | Regra confirmada: o histórico é mantido sempre, sem expiração. A tela ainda aguarda o aval da cliente |

---

<a name="priorizado"></a>

## Backlog Priorizado

Ordem definida pela cliente em 16/10/2026. As três primeiras são as prioridades da sprint atual. As seguintes entram depois, na ordem em que ela as listou.

| Ordem | ID | História | Tamanho | Depende de | Situação |
|---|---|---|---|---|---|
| 1 | [US11](#us11) | Materiais para download | M | US03 (concluída) | Pronta para iniciar. O armazenamento dos arquivos depende da [D05](#duvidas) |
| 2 | [US10](#us10) | Cadastro nacional de líderes | P + M/G | US05 e US03 (concluídas) | Link na página inicial do painel: pronto para iniciar. Entrada automática na galeria: PENDENTE ([D01](#duvidas) a [D04](#duvidas)) |
| 3 | [US14](#us14) | Marcar evento como cancelado ou adiado | P | US09 (concluída) | Pronta para iniciar. Detalhes de exibição na [D11](#duvidas) |
| 4 | [US18](#us18) | Excluir fotos específicas de um álbum | M (G se a [D06](#duvidas) pedir exclusão no Drive) | US07 e US15 (concluídas) | Pronta para iniciar. Alcance da exclusão na [D06](#duvidas) |
| 5 | [US03](#us03) (revisão) | Setores revistos e telas de produtos e de eventos separadas | P a M | — | Separação das telas: pronta. Lista de setores: PENDENTE ([D07](#duvidas)) |
| 6 | [US19](#us19) | Eventos online e presenciais separados | M | Correção do defeito de evento sem coordenadas | Forma de separar no site: PENDENTE ([D10](#duvidas)) |
| 7 | [US20](#us20) | Exclusão de evento com inscritos | P | Decisão da cliente | **Bloqueada** até a [D09](#duvidas). O defeito atual segue aberto |
| — | [US21](#us21) | Pedidos de produto no setor Comercial | Não estimável | [D08](#duvidas) | Escopo PENDENTE. Sem posição definida pela cliente |
| — | [US07](#us07) (restante) | Escolher a capa e reordenar as fotos do álbum | P | — | Necessidade a confirmar ([D12](#duvidas)) |
| — | US08, US12, US13, US16, [US22](#us22) | Demais histórias | — | — | Mantêm a prioridade do formulário de 25/08/2026 |

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
<tr><td><strong>Status</strong></td><td>Concluída na Sprint 1 — <strong>aceita pela cliente em 16/10/2026</strong> ("OK")</td></tr>
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
<tr><td><strong>Status</strong></td><td>Concluída na Sprint 1 — <strong>revisão pedida pela cliente em 16/10/2026</strong> (ver abaixo)</td></tr>
</tbody>
</table>

> **Redução de escopo confirmada.** A cliente marcou apenas três setores. Galeria, Líderes e Voluntários **não** precisam de administrador próprio, o que diminui a matriz de permissões prevista originalmente.

#### Revisão de 16/10/2026 — setores e telas

<table>
<tbody>
<tr><td><strong>Descrição</strong></td><td>Como <em>superadministradora</em>, quero <em>que os setores do painel correspondam à organização real da equipe e que produtos e eventos fiquem em telas separadas</em>, para <em>que cada administrador encontre só o que lhe compete</em>.</td></tr>
<tr><td><strong>Fala da cliente</strong></td><td>Setores: "1 Comercial, e o restante é organizacional". "Separar a tela de produtos da tela de eventos."</td></tr>
<tr><td><strong>Critérios de Aceitação</strong></td><td><strong>CA1.</strong> <strong>Dado</strong> um administrador com acesso a produtos e a eventos, <strong>quando</strong> abre o painel, <strong>então</strong> produtos e eventos aparecem em telas separadas, cada uma com o próprio item de menu. <br><br> <strong>CA2.</strong> <strong>Dado</strong> o setor Comercial, <strong>quando</strong> um administrador é vinculado a ele, <strong>então</strong> acessa as telas comerciais e não acessa as dos setores organizacionais. <br><br> <strong>CA3 — PENDENTE – confirmar com a cliente (<a href="#duvidas">D07</a>).</strong> A lista exata dos setores organizacionais e se o setor Comercial substitui o atual Loja/Produtos. <br><br> <strong>CA4.</strong> As regras da Sprint 1 continuam valendo: a exclusão de conteúdo é restrita à superadministradora e uma pessoa pode responder por mais de um setor.</td></tr>
<tr><td><strong>Tamanho</strong></td><td>P (separação das telas) a M (se a lista de setores mudar os papéis no Keycloak)</td></tr>
<tr><td><strong>Dependências</strong></td><td>Resposta da <a href="#duvidas">D07</a> para o CA2 e o CA3. A <a href="#us21">US21</a> (pedidos de produto) depende do setor Comercial</td></tr>
<tr><td><strong>Prioridade</strong></td><td>5ª na ordem da cliente — depois da sprint atual</td></tr>
</tbody>
</table>

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
<tr><td><strong>Status</strong></td><td>Concluída e no ar desde 05/10/2026 — <strong>aceita pela cliente em 16/10/2026</strong> ("OK")</td></tr>
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
<tr><td><strong>Status</strong></td><td>Concluída e no ar desde 05/10/2026 — <strong>aceita pela cliente em 16/10/2026</strong> ("OK")</td></tr>
</tbody>
</table>

> **Atenção à US10.** Em 16/10/2026 a cliente pediu que quem tiver cargo de diretor nacional entre **automaticamente** na galeria de diretores. Ainda não se sabe se é a galeria de atuais ou a de anteriores, e a de anteriores hoje usa marcação manual. Até a resposta das [D01 e D02](#duvidas), a regra desta história não muda.

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

### Prioridades definidas pela cliente em 16/10/2026

As três histórias abaixo são as prioridades 1, 2 e 3 da cliente e formam o escopo da sprint atual.

<a name="us11"></a>

### US11 — Materiais para download

<table>
<tbody>
<tr><td><strong>ID</strong></td><td>US11</td></tr>
<tr><td><strong>Rastreabilidade</strong></td><td>RF54</td></tr>
<tr><td><strong>Tema</strong></td><td>Experiência do Usuário e Engajamento</td></tr>
<tr><td><strong>Descrição</strong></td><td>Como <em>jovem, adolescente ou líder</em>, quero <em>baixar no site os vídeos, as fotos e os documentos da organização</em>, para <em>ter o material sem precisar pedir a alguém</em>.</td></tr>
<tr><td><strong>Fala da cliente</strong></td><td>25/08: <em>"Eventos, missão, visão e valores e material de download."</em> <br> 16/10: tipos "vídeo, foto e documentos"; "só o superadmin envia"; acesso aberto, "todas as pessoas podem baixar, sem login".</td></tr>
<tr><td><strong>Critérios de Aceitação</strong></td><td><strong>CA1.</strong> <strong>Dado</strong> que sou a superadministradora no painel, <strong>quando</strong> cadastro um material do tipo vídeo, foto ou documento, <strong>então</strong> ele passa a aparecer na área pública de materiais, identificado pelo tipo. <br><br> <strong>CA2.</strong> <strong>Dado</strong> um administrador de setor, <strong>quando</strong> acessa o painel, <strong>então</strong> não vê as opções de enviar, editar ou excluir materiais, e o servidor recusa essas ações se forem tentadas diretamente. <br><br> <strong>CA3.</strong> <strong>Dado</strong> um visitante sem login, <strong>quando</strong> abre a área de materiais a partir da página inicial, <strong>então</strong> vê a lista e baixa qualquer material sem que o site peça para entrar. <br><br> <strong>CA4.</strong> <strong>Dado</strong> um material cadastrado, <strong>quando</strong> a superadministradora o exclui, <strong>então</strong> ele deixa de aparecer na área pública. <br><br> <strong>CA5 — PENDENTE – confirmar com a cliente (<a href="#duvidas">D05</a>).</strong> Se os arquivos são enviados direto ao site ou ficam no Drive com o link cadastrado, e quais formatos de documento são aceitos.</td></tr>
<tr><td><strong>Tamanho</strong></td><td>M. Sobe para G se a D05 pedir envio direto de vídeos ao servidor, porque exige armazenamento e limite de tamanho no back-end</td></tr>
<tr><td><strong>Dependências</strong></td><td>US03 (perfil de superadministradora — concluída). Resposta da D05 para o armazenamento</td></tr>
<tr><td><strong>Prioridade</strong></td><td>Alta — <strong>1ª na ordem da cliente</strong></td></tr>
<tr><td><strong>Status</strong></td><td>Refinada em 16/10/2026 — pronta para a sprint atual</td></tr>
</tbody>
</table>

> **Desmembramento.** A versão anterior desta história juntava materiais para download com missão, visão e valores. A reunião de 16/10 tratou só dos materiais, que viraram a prioridade 1. O conteúdo institucional passou para a [US22](#us22), sem mudança de escopo. A área de materiais **não existe** no sistema atual.

<a name="us10"></a>

### US10 — Cadastro nacional de líderes

<table>
<tbody>
<tr><td><strong>ID</strong></td><td>US10</td></tr>
<tr><td><strong>Rastreabilidade</strong></td><td>RF53, RF03</td></tr>
<tr><td><strong>Tema</strong></td><td>Governança e Operações</td></tr>
<tr><td><strong>Descrição</strong></td><td>Como <em>superadministradora</em>, quero <em>que cada líder do país se cadastre sozinho por um link externo, informando o cargo, e que os diretores nacionais apareçam na galeria do site</em>, para <em>manter o cadastro de mais de 2500 líderes sem digitar um por um</em>.</td></tr>
<tr><td><strong>Fala da cliente</strong></td><td>Escala: "~1050 igrejas, mais de 2500 líderes". "Cada líder se cadastra sozinho por um link externo, informando o cargo, e os dados vão para uma planilha." O link fica "fixado na home do superadmin". "Quem tiver cargo 'diretor' e for nacional entra automaticamente na galeria de diretores." Trecho quase ilegível: "anteriores ... se um novo diretor for cadastrado. Os 2 nomes são diretores".</td></tr>
<tr><td><strong>Critérios de Aceitação</strong></td><td><strong>CA1.</strong> <strong>Dado</strong> que sou a superadministradora, <strong>quando</strong> abro a página inicial do painel, <strong>então</strong> vejo, fixado, o link externo de cadastro de líderes, e posso abri-lo e copiá-lo para divulgar. <br><br> <strong>CA2.</strong> <strong>Dado</strong> um administrador de setor, <strong>quando</strong> abre a página inicial do painel, <strong>então</strong> o link não aparece. <br><br> <strong>CA3.</strong> <strong>Dado</strong> que o link de cadastro mudou, <strong>quando</strong> a superadministradora informa o novo endereço no painel, <strong>então</strong> o link fixado passa a ser o novo, sem depender da equipe técnica. <br><br> <strong>CA4 — PENDENTE – confirmar com a cliente (<a href="#duvidas">D02</a>, <a href="#duvidas">D03</a>).</strong> <strong>Dado</strong> um líder cadastrado com cargo de diretor e abrangência nacional, <strong>quando</strong> o cadastro entra na planilha, <strong>então</strong> ele aparece automaticamente na galeria de diretores. Falta saber qual galeria (atuais ou anteriores) e se o nome aparece direto ou depois de alguém conferir. <br><br> <strong>CA5.</strong> <strong>Dado</strong> um líder com outro cargo, ou diretor não nacional, <strong>quando</strong> se cadastra, <strong>então</strong> não entra na galeria. <br><br> <strong>CA6 — PENDENTE – confirmar com a cliente (<a href="#duvidas">D01</a>).</strong> <strong>Dado</strong> que já existe um diretor nacional na galeria, <strong>quando</strong> um novo diretor é cadastrado, <strong>então</strong> o anterior continua aparecendo e os dois constam como diretores. Esta é a leitura da equipe do trecho quase ilegível, ainda não confirmada. <br><br> <strong>CA7 — PENDENTE – confirmar com a cliente (<a href="#duvidas">D04</a>).</strong> Se o formulário e a planilha já existem e quais dados o líder preenche além do cargo.</td></tr>
<tr><td><strong>Tamanho</strong></td><td>P para o link no painel (CA1 a CA3). M ou G para a entrada automática na galeria (CA4 a CA6): G se o site precisar ler a planilha com mais de 2500 linhas</td></tr>
<tr><td><strong>Dependências</strong></td><td>US05 (galeria de diretores — concluída), US03 (perfil de superadministradora — concluída). Respostas das D01 a D04 para o CA4 a CA7</td></tr>
<tr><td><strong>Prioridade</strong></td><td>Alta — <strong>2ª na ordem da cliente</strong></td></tr>
<tr><td><strong>Status</strong></td><td>Refinada em 16/10/2026 — CA1 a CA3 prontos para a sprint atual; CA4 a CA7 aguardam a cliente</td></tr>
</tbody>
</table>

> **O cadastro em si fica fora do site.** Pela resposta da cliente, o formulário e a planilha são externos. O site só fixa o link e, se confirmado, leva os diretores nacionais à galeria. Isso substitui os critérios provisórios da versão anterior (cadastro por região, consulta restrita), que não foram citados na reunião.

> **Risco registrado pela equipe.** O cargo é informado pelo próprio líder. Se a entrada na galeria pública for automática, qualquer pessoa com o link pode se declarar "diretor nacional" e aparecer no site. Por isso a D03 pergunta se alguém confere antes. A planilha também reúne dados pessoais de mais de 2500 pessoas, e o acesso a ela deve ficar restrito.

<a name="us14"></a>

### US14 — Marcar evento como cancelado ou adiado

<table>
<tbody>
<tr><td><strong>ID</strong></td><td>US14</td></tr>
<tr><td><strong>Rastreabilidade</strong></td><td>RF12</td></tr>
<tr><td><strong>Tema</strong></td><td>Gestão de Eventos</td></tr>
<tr><td><strong>Descrição</strong></td><td>Como <em>administradora de eventos</em>, quero <em>marcar um evento como cancelado ou adiado</em>, para <em>que quem acompanha o site saiba da mudança sem que o evento e suas inscrições sejam apagados</em>.</td></tr>
<tr><td><strong>Fala da cliente</strong></td><td>Nota 4 no formulário de 25/08. Em 16/10, colocada pela cliente como <strong>3ª prioridade</strong>. Sobre o histórico: "Mantenha o histórico sempre."</td></tr>
<tr><td><strong>Critérios de Aceitação</strong></td><td><strong>CA1.</strong> <strong>Dado</strong> que sou administradora do setor Agenda/Eventos ou superadministradora, <strong>quando</strong> edito um evento, <strong>então</strong> posso marcá-lo como "Cancelado" ou "Adiado" e desfazer a marcação. <br><br> <strong>CA2.</strong> <strong>Dado</strong> um evento marcado como cancelado ou adiado, <strong>quando</strong> o visitante o vê na agenda, no calendário, na página inicial ou na página do evento, <strong>então</strong> um aviso visível informa a situação. <br><br> <strong>CA3.</strong> <strong>Dado</strong> um evento marcado como cancelado ou adiado, <strong>quando</strong> a marcação é salva, <strong>então</strong> o evento, as inscrições e as fotos continuam guardados. Nada é apagado. <br><br> <strong>CA4 — PENDENTE – confirmar com a cliente (<a href="#duvidas">D11</a>).</strong> Se o evento cancelado continua aparecendo no site com o aviso, se o adiado mostra a nova data e se os botões de inscrição somem enquanto o evento está cancelado ou adiado. <br><br> <strong>CA5 — PENDENTE – confirmar com a cliente (<a href="#duvidas">D11</a>).</strong> Se os inscritos precisam ser avisados pelo site ou se a equipe avisa por fora.</td></tr>
<tr><td><strong>Tamanho</strong></td><td>P</td></tr>
<tr><td><strong>Dependências</strong></td><td>US03 (setor Agenda/Eventos), US09 (botões de inscrição) — ambas concluídas. Resposta da D11 para o CA4 e o CA5</td></tr>
<tr><td><strong>Prioridade</strong></td><td>Alta — <strong>3ª na ordem da cliente</strong> (antes Baixa)</td></tr>
<tr><td><strong>Status</strong></td><td>Refinada em 16/10/2026 — CA1 a CA3 prontos para a sprint atual</td></tr>
</tbody>
</table>

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
<tr><td><strong>Status</strong></td><td>Front-end concluído na Sprint 2 — <strong>álbuns por evento aceitos pela cliente em 16/10/2026</strong> ("OK"). Aguarda publicação. Capa, reordenação e inclusão simplificada ainda não entregues; a necessidade de capa e reordenação está na <a href="#duvidas">D12</a></td></tr>
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
<tr><td><strong>Responsáveis (front-end)</strong></td><td><a href="https://github.com/daviRolvr">Davi Emanuel Ribeiro de Oliveira</a>, <a href="https://github.com/R-enanVieira">Renan Vieira Guedes</a>, <a href="https://github.com/JoaoPedro2206">João Pedro Ferreira Moraes</a></td></tr>
<tr><td><strong>Status</strong></td><td>Concluída na Sprint 2 — os três critérios atendidos, com front e back-end integrados. <strong>Aceita pela cliente em 16/10/2026</strong> ("OK", texto dos botões aprovado). Aguarda publicação</td></tr>
</tbody>
</table>

> **Requisito não previsto no catálogo.** A cliente escreveu: *"Nessa etapa podia ter a inscrição do evento e também de voluntários, seriam 2 links então."* Até esta entrega o sistema contemplava **apenas** o fluxo de voluntariado — e o botão "Inscreva-se" das telas públicas abria justamente o formulário de voluntariado, que é outra inscrição.

> A US10 (cadastro nacional de líderes) e a US11 (materiais para download) subiram para a [Prioridade Alta](#alta) na reunião de 16/10/2026.

<a name="us18"></a>

### US18 — Excluir fotos específicas de um álbum

<table>
<tbody>
<tr><td><strong>ID</strong></td><td>US18</td></tr>
<tr><td><strong>Rastreabilidade</strong></td><td>RF55 <em>(novo)</em>, RF07</td></tr>
<tr><td><strong>Tema</strong></td><td>Experiência do Usuário e Engajamento</td></tr>
<tr><td><strong>Descrição</strong></td><td>Como <em>superadministradora</em>, quero <em>excluir fotos específicas do álbum de um evento</em>, para <em>atender na hora o responsável que pede a remoção da imagem de um menor, sem tirar o álbum inteiro do ar</em>.</td></tr>
<tr><td><strong>Fala da cliente</strong></td><td>Autorização de imagem: existe "por evento". "Ninguém confere as fotos antes de subir. Só o fotógrafo é responsável." Quando um responsável pede a remoção, "a equipe verifica o álbum no Drive". O sistema precisa de um botão para "excluir fotos específicas de um álbum".</td></tr>
<tr><td><strong>Critérios de Aceitação</strong></td><td><strong>CA1.</strong> <strong>Dado</strong> que sou a superadministradora, <strong>quando</strong> abro no painel o álbum de um evento, <strong>então</strong> vejo as fotos do álbum e posso selecionar uma ou mais. <br><br> <strong>CA2.</strong> <strong>Dado</strong> fotos selecionadas, <strong>quando</strong> confirmo a exclusão, <strong>então</strong> elas deixam de aparecer na galeria pública, na página do evento e na seção "Já aconteceram", e a contagem de fotos do álbum diminui. <br><br> <strong>CA3.</strong> <strong>Dado</strong> que a foto excluída era a capa do álbum (hoje, a primeira foto), <strong>quando</strong> a exclusão é concluída, <strong>então</strong> a capa passa a ser a foto seguinte. <br><br> <strong>CA4.</strong> <strong>Dado</strong> uma exclusão, <strong>quando</strong> ela é concluída, <strong>então</strong> as demais fotos do álbum e os álbuns dos outros eventos não são afetados. <br><br> <strong>CA5.</strong> <strong>Dado</strong> um administrador de setor, <strong>quando</strong> abre o álbum, <strong>então</strong> não vê a opção de excluir, e o servidor recusa a ação se tentada. A exclusão de conteúdo é restrita à superadministradora (US03). <br><br> <strong>CA6 — PENDENTE – confirmar com a cliente (<a href="#duvidas">D06</a>).</strong> Se a foto excluída some só do site ou também é apagada da pasta do Drive.</td></tr>
<tr><td><strong>Tamanho</strong></td><td>M. G se a D06 exigir apagar no Drive, porque o sistema precisará de permissão de escrita na pasta</td></tr>
<tr><td><strong>Dependências</strong></td><td>US07 (álbuns por evento) e US15 (histórico) — concluídas. US03 (regra de exclusão)</td></tr>
<tr><td><strong>Prioridade</strong></td><td>Média — <strong>4ª na ordem da cliente</strong>, primeira depois da sprint atual</td></tr>
<tr><td><strong>Status</strong></td><td>História nova (16/10/2026) — substitui o escopo em aberto da US17</td></tr>
</tbody>
</table>

<a name="us19"></a>

### US19 — Eventos online e presenciais separados

<table>
<tbody>
<tr><td><strong>ID</strong></td><td>US19</td></tr>
<tr><td><strong>Rastreabilidade</strong></td><td>RF56 <em>(novo)</em></td></tr>
<tr><td><strong>Tema</strong></td><td>Gestão de Eventos</td></tr>
<tr><td><strong>Descrição</strong></td><td>Como <em>visitante</em>, quero <em>ver separados os eventos online e os presenciais</em>, para <em>encontrar logo os eventos de que posso participar</em>.</td></tr>
<tr><td><strong>Fala da cliente</strong></td><td>Anotação livre: "Separar eventos ONLINE e PRESENCIAIS."</td></tr>
<tr><td><strong>Critérios de Aceitação</strong></td><td><strong>CA1.</strong> <strong>Dado</strong> que sou administradora do setor Agenda/Eventos, <strong>quando</strong> crio ou edito um evento, <strong>então</strong> informo se ele é online ou presencial. <br><br> <strong>CA2.</strong> <strong>Dado</strong> eventos das duas modalidades, <strong>quando</strong> o visitante abre a agenda, <strong>então</strong> os online e os presenciais aparecem separados. <br><br> <strong>CA3.</strong> <strong>Dado</strong> um evento online sem endereço e sem coordenadas, <strong>quando</strong> é salvo, <strong>então</strong> o cadastro é aceito e a listagem pública continua funcionando. <br><br> <strong>CA4.</strong> <strong>Dado</strong> eventos já cadastrados antes desta história, <strong>quando</strong> ela entra no ar, <strong>então</strong> eles são tratados como presenciais até serem editados <em>(proposta da equipe)</em>. <br><br> <strong>CA5 — PENDENTE – confirmar com a cliente (<a href="#duvidas">D10</a>).</strong> A forma de separar no site (duas seções, filtro ou selo no card) e se o evento online precisa de um link de transmissão.</td></tr>
<tr><td><strong>Tamanho</strong></td><td>M</td></tr>
<tr><td><strong>Dependências</strong></td><td>Correção do defeito registrado na Sprint 2: evento sem latitude e longitude derruba a listagem com erro 500. Sem ela, o CA3 não é atendido. Resposta da D10 para o CA2 e o CA5</td></tr>
<tr><td><strong>Prioridade</strong></td><td>Média — <strong>6ª na ordem da cliente</strong></td></tr>
<tr><td><strong>Status</strong></td><td>História nova (16/10/2026)</td></tr>
</tbody>
</table>

<a name="us20"></a>

### US20 — Exclusão de evento com inscritos

<table>
<tbody>
<tr><td><strong>ID</strong></td><td>US20</td></tr>
<tr><td><strong>Rastreabilidade</strong></td><td>Defeito QA08 (Sprint 2), RF13</td></tr>
<tr><td><strong>Tema</strong></td><td>Gestão de Eventos</td></tr>
<tr><td><strong>Descrição</strong></td><td>Como <em>superadministradora</em>, quero <em>que a exclusão de um evento que já tem inscritos tenha um comportamento previsível</em>, para <em>não ficar com erro na tela e saber o que acontece com o histórico de inscrições</em>.</td></tr>
<tr><td><strong>Fala da cliente</strong></td><td>Marcou um "X" ao lado de "Apagar tudo junto" (o evento some com todo o histórico de inscrições). Sobre o histórico: "Mantenha o histórico sempre."</td></tr>
<tr><td><strong>Decisão</strong></td><td><strong>PENDENTE – confirmar com a cliente (<a href="#duvidas">D09</a>).</strong> O "X" pode indicar escolha ou rejeição. Além disso, "Apagar tudo junto" conflita com "manter o histórico sempre". Os critérios das duas opções estão abaixo. Só a que ela confirmar será implementada.</td></tr>
<tr><td><strong>Critérios — Opção A: apagar tudo junto</strong></td><td><strong>A1.</strong> <strong>Dado</strong> um evento com inscrições de participantes ou de voluntários, <strong>quando</strong> a superadministradora pede a exclusão, <strong>então</strong> o sistema informa quantas inscrições serão apagadas e pede confirmação. <br><br> <strong>A2.</strong> <strong>Dado</strong> a confirmação, <strong>quando</strong> a exclusão é processada, <strong>então</strong> o evento, as inscrições e os vínculos com convidados são apagados juntos, e o evento sai da agenda do Google. <br><br> <strong>A3.</strong> <strong>Dado</strong> uma falha no meio da exclusão, <strong>quando</strong> ela ocorre, <strong>então</strong> nada é apagado, inclusive a agenda do Google.</td></tr>
<tr><td><strong>Critérios — Opção B: recusar e preservar o histórico</strong></td><td><strong>B1.</strong> <strong>Dado</strong> um evento com inscrições, <strong>quando</strong> a superadministradora pede a exclusão, <strong>então</strong> o sistema recusa com uma mensagem clara, informando quantas inscrições existem. <br><br> <strong>B2.</strong> <strong>Dado</strong> a recusa, <strong>quando</strong> ela é exibida, <strong>então</strong> nada é apagado, nem o evento nem a agenda do Google. <br><br> <strong>B3.</strong> <strong>Dado</strong> a recusa, <strong>quando</strong> a mensagem é exibida, <strong>então</strong> ela orienta a marcar o evento como cancelado (US14) <em>(sugestão da equipe)</em>.</td></tr>
<tr><td><strong>Critério comum</strong></td><td><strong>Dado</strong> um evento sem nenhuma inscrição, <strong>quando</strong> a superadministradora o exclui, <strong>então</strong> a exclusão funciona como hoje.</td></tr>
<tr><td><strong>Tamanho</strong></td><td>P em qualquer das opções</td></tr>
<tr><td><strong>Dependências</strong></td><td>Resposta da D09. Para a opção B, a US14 (cancelado/adiado)</td></tr>
<tr><td><strong>Prioridade</strong></td><td>Média — <strong>7ª na ordem da cliente</strong>, só depois da confirmação</td></tr>
<tr><td><strong>Status</strong></td><td><strong>Bloqueada</strong> — aguarda a decisão da cliente</td></tr>
</tbody>
</table>

> **O defeito continua aberto.** Hoje, excluir um evento com vínculo responde erro 500 e não apaga nada no banco. Pior: a agenda do Google é apagada antes de o banco recusar (registrado na QA08). Enquanto a D09 não for respondida, nada muda. A mesma falha atinge a exclusão de **convidado** vinculado a evento. Esse caso não foi tratado na reunião e segue com a decisão pendente da Sprint 2.

<a name="us22"></a>

### US22 — Missão, visão e valores

<table>
<tbody>
<tr><td><strong>ID</strong></td><td>US22</td></tr>
<tr><td><strong>Rastreabilidade</strong></td><td>RF25, RF26</td></tr>
<tr><td><strong>Tema</strong></td><td>Experiência do Usuário e Engajamento</td></tr>
<tr><td><strong>Descrição</strong></td><td>Eu, como <em>jovem ou adolescente</em>, desejo <em>encontrar no site a missão, a visão e os valores</em>, para <em>conhecer a organização sem pedir a alguém</em>.</td></tr>
<tr><td><strong>Critérios de Aceitação</strong></td><td>- Conteúdo institucional (missão, visão e valores) publicado e editável pelo painel <br> - Navegação clara a partir da página inicial</td></tr>
<tr><td><strong>Prioridade</strong></td><td>Média</td></tr>
<tr><td><strong>Status</strong></td><td>Desmembrada da US11 em 16/10/2026, sem mudança de escopo. Não tratada na reunião de validação</td></tr>
</tbody>
</table>

> Origem na mesma resposta da US11: *"Eventos, missão, visão e valores e material de download."*

---

<a name="baixa"></a>

## Prioridade Baixa

| ID | História | Rastreabilidade | Justificativa da posição |
|---|---|---|---|
| US12 | Cadastro de produtos com preço, tamanho e disponibilidade | RF32, RF33 | A cliente respondeu *"queremos usar, mas ainda não cadastramos os produtos"* — a loja **permanece no escopo**, mas a venda segue por link externo |
| US13 | Rascunho e duplicação de evento | RF08, RF09 | Nota 4 na grade; não citado no desempate |
| US15<a name="us15"></a> | Histórico de eventos passados com fotos | RF13 | Nota 4; dependia da US07 — **entregue na Sprint 2**, assim que a US07 foi mesclada. Em 16/10/2026 a cliente confirmou: *"Não é preocupação."* O histórico é mantido sempre, sem expiração nem remoção automática. A tela aguarda o aval dela para ir ao ar |
| US16 | Acessibilidade e leitor de tela | RNF02 | Nota 4; sem demanda concreta relatada |
| US17 | Controle de direito de imagem de menores | RF07 | A cliente marcou *"sim, precisa de controle"*. **Escopo definido em 16/10/2026:** a autorização existe por evento, ninguém confere as fotos antes de subir (*"Só o fotógrafo é responsável"*), e o controle necessário é excluir fotos específicas a pedido do responsável. Atendida pela [US18](#us18) |

> A US14 (cancelado ou adiado) subiu para a [Prioridade Alta](#us14) em 16/10/2026.

<a name="us21"></a>

### US21 — Pedidos de produto no setor Comercial

<table>
<tbody>
<tr><td><strong>ID</strong></td><td>US21</td></tr>
<tr><td><strong>Rastreabilidade</strong></td><td>RF57 <em>(novo)</em>, RF35</td></tr>
<tr><td><strong>Tema</strong></td><td>Governança e Operações</td></tr>
<tr><td><strong>Descrição</strong></td><td>Como <em>administrador do setor Comercial</em>, quero <em>gerenciar os pedidos de produto</em>, para <em>PENDENTE – confirmar com a cliente</em>.</td></tr>
<tr><td><strong>Fala da cliente</strong></td><td>Anotação junto do administrador por setor: "+ Pedido do produto".</td></tr>
<tr><td><strong>Critérios de Aceitação</strong></td><td><strong>PENDENTE – confirmar com a cliente (<a href="#duvidas">D08</a>).</strong> A leitura da equipe é de uma gestão de pedidos de produto no setor Comercial. Não há informação para escrever critérios.</td></tr>
<tr><td><strong>Tamanho</strong></td><td>Não estimável até a D08</td></tr>
<tr><td><strong>Dependências</strong></td><td>Revisão da US03 (setor Comercial). Relação com a US12 (cadastro de produtos)</td></tr>
<tr><td><strong>Prioridade</strong></td><td>A definir — não consta da ordem dada pela cliente</td></tr>
<tr><td><strong>Status</strong></td><td>História nova (16/10/2026) — escopo pendente</td></tr>
</tbody>
</table>

> **Possível conflito com decisão anterior.** Na US12 ficou registrado que *"a venda segue por link externo"*. Se "pedido do produto" significar receber pedidos dentro do site, essa decisão muda. A D08 pergunta isso.

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
| Expiração ou remoção automática de eventos passados | RF13 (parcial) | Em 16/10/2026: *"Não é preocupação."* O histórico é mantido sempre |
| Conferência das fotos antes da publicação | RF07 (parcial) | Em 16/10/2026: *"Ninguém confere as fotos antes de subir. Só o fotógrafo é responsável."* O controle pedido é a exclusão posterior ([US18](#us18)) |

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
| US10 | RF53, RF03 | Campo aberto — requisito novo. **2ª prioridade da cliente em 16/10/2026** |
| US11 | RF54 | Campo aberto + grade nota 5. **1ª prioridade da cliente em 16/10/2026** |
| US14 | RF12 | Nota 4 na grade. **3ª prioridade da cliente em 16/10/2026** |
| US18 | RF55, RF07 | Reunião de 16/10/2026 — resposta sobre direito de imagem (refina a US17). 4ª na ordem |
| US03 (revisão) | RF01 | Reunião de 16/10/2026 — setores e telas. 5ª na ordem |
| US19 | RF56 | Reunião de 16/10/2026 — anotação livre. 6ª na ordem |
| US20 | QA08, RF13 | Reunião de 16/10/2026 — exclusão de evento com inscritos. 7ª na ordem, após confirmação |
| US21 | RF57, RF35 | Reunião de 16/10/2026 — anotação "+ Pedido do produto". Sem ordem definida |
| US22 | RF25, RF26 | Desmembrada da US11 |

### Referências indicadas pela cliente

Sites citados como inspiração, úteis para a etapa de prototipação:

- itsbr.com.br
- jesuscopy.com
- dunamismovement.com

---

<a name="duvidas"></a>

## Dúvidas para confirmar com a Pra. Raquel

Pontos da reunião de 16/10/2026 em que a resposta ficou ambígua, ilegível ou incompleta. Cada critério marcado como **PENDENTE** nas histórias aponta para uma destas perguntas. As respostas devem ser pedidas por escrito.

| ID | História | Pergunta | O que fica travado até a resposta |
|---|---|---|---|
| D01 | [US10](#us10) | O trecho "anteriores ... se um novo diretor for cadastrado. Os 2 nomes são diretores" quer dizer que, quando entra um novo diretor, o anterior continua aparecendo e os dois ficam como diretores? | CA6 da US10 |
| D02 | [US10](#us10), [US05](#us05) | "Entra automaticamente na galeria de diretores": é a galeria dos diretores atuais (organograma da página inicial) ou a de diretores anteriores? Hoje a marcação de "anterior" é manual, por decisão de 25/08. | CA4 da US10 |
| D03 | [US10](#us10) | O nome do diretor nacional deve aparecer no site assim que ele preenche o cadastro, ou só depois de alguém da equipe conferir? O cargo é informado pelo próprio líder. | CA4 da US10 e o tamanho dela |
| D04 | [US10](#us10) | O formulário e a planilha de cadastro já existem? Quais dados o líder preenche além do cargo (nome, igreja, região, contato, foto)? Se já existirem, pedir o link. | CA7 da US10 |
| D05 | [US11](#us11) | Os arquivos para download são enviados direto ao site ou ficam no Drive, com o link cadastrado no site? Quais tipos de documento entram (PDF, Word, apresentação)? | CA5 e tamanho da US11 |
| D06 | [US18](#us18) | Ao excluir uma foto pelo site, ela deve sumir só do site ou também ser apagada da pasta do Drive? | CA6 e tamanho da US18 |
| D07 | [US03](#us03) | Qual é a lista exata dos setores? "1 Comercial, e o restante é organizacional": quais são os organizacionais? O Comercial substitui o atual Loja/Produtos? | CA2 e CA3 da revisão da US03 |
| D08 | [US21](#us21) | O que a senhora imagina para o "pedido do produto"? Receber pedidos de quem quer comprar, acompanhar pedidos já feitos, outra coisa? Hoje a compra é feita por link externo. | US21 inteira |
| D09 | [US20](#us20) | Excluir evento que já tem inscritos: o "X" ao lado de "Apagar tudo junto" quer dizer que a senhora escolhe essa opção ou que não a quer? Apagar junto perde o histórico de inscrições, que a senhora pediu para manter sempre. | US20 inteira |
| D10 | [US19](#us19) | Como separar eventos online e presenciais no site: duas seções, um filtro ou uma etiqueta no card? O evento online precisa de link de transmissão? | CA2 e CA5 da US19 |
| D11 | [US14](#us14) | Evento cancelado continua aparecendo no site com o aviso? No adiado, mostramos a nova data? Os botões de inscrição somem? Os inscritos precisam ser avisados pelo site, ou a equipe avisa? | CA4 e CA5 da US14 |
| D12 | [US07](#us07) | Escolher a foto de capa e reordenar as fotos do álbum ainda são necessários, ou o álbum como está é suficiente? | Restante da US07 |

---

<a name="requisitos"></a>

## Requisitos Elicitados

Catálogo completo dos requisitos levantados na elicitação, usado como referência para a coluna **Rastreabilidade** das histórias acima. São **67 requisitos**: 7 originados de relatos diretos da cliente, 54 propostos pela equipe a partir da análise do código-fonte, 3 descobertos nos campos abertos do formulário e 3 surgidos na reunião de validação de 16/10/2026.

Cada requisito possui campo de **Responsável** e de **O que foi feito**. O primeiro identifica quem assumiu o item; o segundo descreve a entrega de forma que possa ser verificada sem abrir o código.

**Legenda de origem:** `C` relato direto da cliente · `C+T` relato confirmado por evidência técnica · `T` achado da equipe na análise do código · `S` sugestão da equipe · `A` resposta aberta do formulário · `R` reunião de validação de 16/10/2026

### Relatos originais da cliente

| ID | Requisito | Origem | Situação | Responsável | O que foi feito |
|---|---|---|---|---|---|
| RF01 | Administradores com permissão separada por setor | C+T | US03 — Alta | QA: [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin)<br>Back-end: [Luiz Henrique Guimarães Soares](https://github.com/luizh-gsoares), Daniel dos Santos Barros de Sousa ([Daniel dos Santos Barros de Sousa](https://github.com/daniel-de-sousa))<br>Front-end: [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206), [Renan Vieira Guedes](https://github.com/R-enanVieira) | **QA:** 44 testes de autorização por papel: matriz de 13 rotas, negação de acesso e escalonamento de privilégio. Duas vulnerabilidades documentadas na [Matriz de Autorização](matriz-autorizacao.md), uma delas corrigida.<br>**Back-end:** desenvolvimento do controle de permissões por setor, incluindo filtros de acesso, regras de autorização e o fluxo de gestão de administradores.<br>**Front-end:** três setores lidos dos papéis do Keycloak, com menu, painel e rotas filtrados por setor; exclusão de conteúdo restrita à superadministradora, recusada também na camada de serviço; tela de gestão de administradores.<br>**QA (Sprint 2):** a matriz de autorização passou a ser lida das rotas registradas na aplicação, cobrindo as 49 rotas — rota nova sem decisão de acesso, escrita sem guarda ou exclusão sem superadministrador fazem o teste falhar. Com isso, a listagem de inscritos de cada evento, que era pública, passou a exigir o setor de inscrições. As duas guardas de acesso passaram a recusar com 403 papéis em formato inesperado, que antes concediam acesso ou respondiam 500. <br><br>**QA:** a matriz de autorização passou a ser lida das rotas registradas na aplicação, cobrindo as 49 rotas — rota nova sem decisão de acesso, escrita sem guarda ou exclusão sem superadministrador fazem o teste falhar. Com isso, a listagem de inscritos de cada evento, que era pública e devolvia nome e e-mail, passou a exigir o setor de inscrições. As duas guardas passaram a recusar com 403 papéis em formato inesperado, que antes concediam acesso ou respondiam 500. |
| RF02 | Cadastrar e editar líderes atuais pelo painel | C+T | US05 — Alta | Implementação (front-end): [Gabriel Lopes de Amorim](https://github.com/BrzGab), [Maria Samara Alves Silva](https://github.com/SamaraAlvess), [João Vitor Alves Viana](https://github.com/Joaovitor045)<br>Implementação (back-end): [Filipe Carvalho da Silva](https://github.com/Filipe-002), [João Rodrigues](https://github.com/JpRodrigues2), [Júlia dos Reis Teixeira Massuda](https://github.com/JuliaReis18) | **Implementação (front-end):** tela "Diretores & Líderes" no painel, restrita à superadministradora — o administrador comum não vê o item no menu e é redirecionado ao acessar a rota. Listagem em abas (atuais e anteriores), cadastro, edição e exclusão com confirmação. Campos: nome, cargo, foto (link do Drive com pré-visualização), região, redes sociais, mini-biografia, ordem de exibição e marcação manual de "diretor anterior". A seção "Nosso Organograma" da página inicial passou a ler os líderes da API: deixaram de estar fixos no código-fonte. 12 testes E2E.<br>**Implementação (back-end):** persistência dos campos `regiao`, `bio`, `redes_sociais` e `gestao` no cadastro de líder — que antes eram enviados pelo front-end e ignorados pelo schema —, endpoint de perfil exposto na API com escrita restrita à superadministradora, e padronização dos erros das rotas de líderes. Cobertura de testes unitários e de integração para schema, repositório, serviço, controller e autorização.<br>**Protótipo:** seis telas no Figma do projeto, feitas pelo trio — [listagem](https://www.figma.com/design/a6mGpOqwc1ZCeVTOIWtyxo/IDB_Jovem?node-id=1428-1476), [anteriores](https://www.figma.com/design/a6mGpOqwc1ZCeVTOIWtyxo/IDB_Jovem?node-id=1428-1648), [cadastrar](https://www.figma.com/design/a6mGpOqwc1ZCeVTOIWtyxo/IDB_Jovem?node-id=1390-1369), [editar](https://www.figma.com/design/a6mGpOqwc1ZCeVTOIWtyxo/IDB_Jovem?node-id=1428-1771), [excluir](https://www.figma.com/design/a6mGpOqwc1ZCeVTOIWtyxo/IDB_Jovem?node-id=1438-1574) e [após exclusão](https://www.figma.com/design/a6mGpOqwc1ZCeVTOIWtyxo/IDB_Jovem?node-id=1440-1684). <br>**Produção (Sprint 2):** publicado no site em 05/10/2026 pelo [PR #2 da organização](https://github.com/idbjovemnacional/IDB_Jovem-Teen/pull/2), enviado pelo trio do front-end. |
| RF03 | Galeria de diretores anteriores editável | C | US05 — Alta (restrita ao cargo nacional) | Implementação: [Gabriel Lopes de Amorim](https://github.com/BrzGab), [Maria Samara Alves Silva](https://github.com/SamaraAlvess), [João Vitor Alves Viana](https://github.com/Joaovitor045) | **Implementação:** aba "Diretores anteriores" no painel e "Galeria de Diretores" na página inicial listam apenas quem foi marcado manualmente como anterior **e** tem cargo nacional. O formulário avisa quando um líder regional marcado como anterior não entrará na galeria e coleta o período de gestão exibido no card ("Gestão 2020 – 2023"). [PR #2](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/2).<br>**Protótipo:** tela [US 5 - SuperAdmin - Diretores/Líderes - Anteriores](https://www.figma.com/design/a6mGpOqwc1ZCeVTOIWtyxo/IDB_Jovem?node-id=1428-1648) no Figma do projeto, feita pelo trio. |
| RF04 | Corrigir foto duplicada do Pr. Áquila | C+T | US04 — Alta (causa: tradução automática) | QA e implementação: [Rivadalvio Joaquim da Silva Filho](https://github.com/RivaFilho)<br>Implementação: [Gabriel Lopes de Amorim](https://github.com/BrzGab), [Maria Samara Alves Silva](https://github.com/SamaraAlvess), [João Vitor Alves Viana](https://github.com/Joaovitor045) | **QA:** 4 testes de ausência de líder duplicado — nas duas abas da seção, repetição de imagem e estabilidade da contagem ao alternar entre elas. Rodados antes da correção, os quatro já passavam: sem tradutor ativo as listas não duplicam, o que confirma que o defeito é artefato da tradução e não cadastro repetido.<br>**Implementação:** resolvido junto com RF06 — a duplicação some com a correção do idioma. No [PR #6](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/6), os nomes dos líderes (página inicial e painel) receberam `translate="no"`, e um teste de regressão percorre as duas abas da seção verificando que não há nome nem foto repetidos. |
| RF05 | Evento com múltiplos dias | C+T | US01 — Alta | QA: [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin)<br>Back-end: Daniel dos Santos Barros de Sousa ([Daniel dos Santos Barros de Sousa](https://github.com/daniel-de-sousa)), [Luiz Henrique Guimarães Soares](https://github.com/luizh-gsoares)<br>Front-end: [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206), [Renan Vieira Guedes](https://github.com/R-enanVieira) | **QA:** 16 testes de faixa de datas: até três dias, dias não consecutivos, virada de mês e de ano, rejeição de datas invertidas.<br>**Back-end:** lógica de eventos com múltiplos dias, validação de datas e refinamento do comportamento no fluxo de cadastro do calendário.<br>**Front-end:** card único no calendário com a faixa de datas; evento em andamento deixa de sumir do site; evento que atravessa a virada aparece nos dois meses; seleção de dias não consecutivos no formulário. |
| RF06 | Corrigir tradução automática indevida no celular | C+T | US04 — Alta | QA e implementação: [Rivadalvio Joaquim da Silva Filho](https://github.com/RivaFilho)<br>Implementação: [Gabriel Lopes de Amorim](https://github.com/BrzGab), [Maria Samara Alves Silva](https://github.com/SamaraAlvess), [João Vitor Alves Viana](https://github.com/Joaovitor045) | **QA:** 2 testes verificando que o documento declara `pt-BR`, na página inicial e em rotas internas. Ambos falhavam antes da correção. Suíte de `tests/public` e `tests/layout` reexecutada: 108 passando.<br>**Implementação:** `lang="en"` corrigido para `pt-BR` no `index.html`. O navegador deixa de oferecer tradução automática, e com isso para de reescrever os nós de texto e quebrar a reconciliação do React.<br>**Complemento ([PR #6](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/6)):** meta `notranslate`, para que o Chrome não ofereça tradução mesmo com o celular em outro idioma; `translate="no"` no título da marca; `lang` e nome do aplicativo no `site.webmanifest`; 4 testes de regressão — idioma do documento, meta, nomes não traduzíveis e ausência de duplicação. <br>**Produção (Sprint 2):** publicado no site em 05/10/2026 pelo [PR #2 da organização](https://github.com/idbjovemnacional/IDB_Jovem-Teen/pull/2), enviado pelo trio do front-end. |
| RF07 | Corrigir problemas da galeria de fotos | C | US07 — Média | Implementação (front-end): [Gabriel Lopes de Amorim](https://github.com/BrzGab), [Maria Samara Alves Silva](https://github.com/SamaraAlvess), [João Vitor Alves Viana](https://github.com/Joaovitor045) | **Implementação (front-end, Sprint 2):** a galeria pública deixou de misturar as fotos de eventos diferentes numa grade única, que era o problema relatado pela cliente. As fotos passaram a ser agrupadas por evento (ver RF23). Ainda não atendido: a inclusão de fotos pelo painel segue o processo atual, pelo link do Drive do evento. [PR #15](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/15). |

### Gestão de eventos

| ID | Requisito | Origem | Situação | Responsável | O que foi feito |
|---|---|---|---|---|---|
| RF08 | Rascunho: criar evento e publicar depois | S | US13 — Baixa | | |
| RF09 | Duplicar evento anterior para nova edição | S | US13 — Baixa | | |
| RF10 | Eventos recorrentes automáticos | S | Backlog | | |
| RF11 | Campos novos: vagas, valor, faixa etária, prazo, o que levar | S | US02 — Alta | QA: [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin)<br>Implementação (US01): [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206), [Renan Vieira Guedes](https://github.com/R-enanVieira)<br>Implementação (US02): [Gabriel Lopes de Amorim](https://github.com/BrzGab), [Maria Samara Alves Silva](https://github.com/SamaraAlvess), [João Vitor Alves Viana](https://github.com/Joaovitor045) | **QA:** testes de campos complementares: link de formulário, link de galeria e campos opcionais ausentes não bloqueiam a criação.<br>**Implementação (parcial, via US01):** reestruturação do formulário de evento e da validação de datas, que a US02 estende com os cinco campos novos.<br>**Implementação (US02):** cinco campos novos no formulário de criação e edição de evento — quantidade de vagas, contato do responsável, valor/investimento, link de pagamento e regulamento (link de PDF no Drive, no mesmo padrão da capa e da galeria) — carregados a partir do evento ao editar ([commit 78de466](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/commit/78de466d82e35f9df0b49f09b1fd59b21fb8f2fc)).<br>**Pendência:** mapear os campos em `eventService.toApiEvent`, criar as colunas no back-end e exibir o regulamento na página pública do evento.<br>**Protótipo:** tela [US 2 - Admin - Eventos - Criação de Eventos](https://www.figma.com/design/a6mGpOqwc1ZCeVTOIWtyxo/IDB_Jovem?node-id=388-1990) no Figma do projeto, feita pelo trio. |
| RF12 | Marcar evento como cancelado ou adiado | S | US14 — Alta (3ª prioridade da cliente, 16/10/2026) | | |
| RF13 | Histórico de eventos passados com fotos | S | US15 — Baixa | [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [Renan Vieira Guedes](https://github.com/R-enanVieira), [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206) | **Entregue.** Um evento encerrado sumia do site público inteiro — a agenda, o mapa e a página inicial filtram todos pelo que ainda não terminou —, e as fotos ficavam na galeria soltas do evento que as originou. A página de eventos ganhou a seção "Já aconteceram", com os eventos passados do mais recente para o mais antigo: capa do álbum, contagem de fotos, data e local, e o card levando à página do evento, onde o álbum da US07 já aparece. A contagem só é exibida quando há fotos, e a seção se esconde quando não existe evento encerrado ou quando a busca falha. 5 testes E2E.<br>**Sem dependência do back-end:** os eventos já vinham de `GET /evento/` e as fotos de `GET /evento/{id}/galeria`. A história dependia da US07, mesclada nesta sprint — foi o que a destravou. |
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
| RF23 | Álbum de fotos separado por evento | T | US07 — Média | Implementação (front-end): [Gabriel Lopes de Amorim](https://github.com/BrzGab), [Maria Samara Alves Silva](https://github.com/SamaraAlvess), [João Vitor Alves Viana](https://github.com/Joaovitor045) | **Implementação (front-end, Sprint 2):** a página `/galeria` lista um álbum por evento com galeria cadastrada, cada um com capa, nome, local e quantidade de fotos. Ao abrir um álbum, aparecem só as fotos daquele evento, e o botão de voltar retorna à lista. As fotos vêm da galeria do Drive de cada evento; sem nenhuma foto cadastrada, a página mostra fotos de exemplo. Pendente: escolher a foto de capa (hoje é a primeira do álbum) e reordenar as imagens. [PR #15](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Teen/pull/15). |
| RF24 | Vídeos além de fotos na galeria | S | Backlog | | |
| RF25 | Editar textos institucionais pelo painel | S | US22 — Média | | |
| RF26 | Seção de avisos e comunicados na home | S | US22 — Média | | |
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
| RF47 | Gerenciar bandas e palestrantes pelo painel | T | US06 — Alta | QA: [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin)<br>Implementação: [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206), [Renan Vieira Guedes](https://github.com/R-enanVieira) |**Implementação:** Tela de convidados no painel, com busca por nome e função, cadastro, edição e exclusão. Construída sobre o CRUD que a API já oferecia e que nenhuma tela alcançava. A função (banda, pregador, convidado) é derivada do campo de texto livre da API, sem descaracterizar o que já estava cadastrado.<br><br>**QA (Sprint 2):** edição de convidados coberta, inclusive a persistência. Registrado defeito em aberto: excluir convidado vinculado a evento responde 500 e não apaga nada — aguarda decisão entre apagar os vínculos junto ou recusar com 409. |
| RF48 | Exibir convidados na página pública do evento | S | US06 — Alta | QA: [Rivadalvio Joaquim da Silva Filho](https://github.com/RivaFilho)<br>Implementação: [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206), [Renan Vieira Guedes](https://github.com/R-enanVieira) | **QA:** teste que percorre a página do evento verificando que cada convidado aparece sob a função correta e que grupo sem ninguém não é renderizado — a regressão a evitar é uma banda voltar a ser anunciada como palestrante.<br>**Implementação:** convidados separados por função na página do evento: antes o título era fixo em "Palestrantes" e uma banda aparecia anunciada como palestrante. Grupo sem ninguém não é exibido. |
| RF49 | Reaproveitar convidado em novo evento | S | US06 — Alta | QA: [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin)<br>Implementação: [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206), [Renan Vieira Guedes](https://github.com/R-enanVieira) |**Implementação:** No formulário do evento, o texto livre deu lugar à seleção dos já cadastrados, com cadastro rápido para quem ainda não existe. O vínculo passou a ser por identificador: antes era resolvido comparando nomes, e corrigir a grafia de um nome criava um cadastro novo.<br><br>**QA (Sprint 2):** vínculo por identificador testado contra banco real — o mesmo convidado em dois eventos, eventos que não misturam convidados, duplicata barrada pela chave composta e desvínculo que preserva o cadastro. |
| RF50 | Vincular convidado a atividade da programação | S | US06 — Alta | [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206), [Renan Vieira Guedes](https://github.com/R-enanVieira) | **Não entregue.** Exige tabela nova no back-end ligando convidado a atividade — a `atividade` só conhece o evento. Levado a refinamento em vez de virar tela que não guarda nada. |
| RF51 | Histórico de participações por convidado | S | Backlog | [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206), [Renan Vieira Guedes](https://github.com/R-enanVieira) | **Parcial.** A listagem de convidados mostra em quantos eventos cada um já esteve, e a confirmação de exclusão avisa quando o convidado está anunciado em algum. Não há tela de histórico por convidado — o dado existe, falta a visualização dedicada. |

### Requisitos surgidos das respostas abertas

| ID | Requisito | Origem | Situação | Responsável | O que foi feito |
|---|---|---|---|---|---|
| RF52 | Inscrição de participante, separada da de voluntário | A | US09 — Média | Front-end: [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [Renan Vieira Guedes](https://github.com/R-enanVieira), [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206) | **Entregue no front.** O evento passou a ter dois fluxos de inscrição independentes: dois campos de link no formulário do painel e duas listagens em abas na tela de inscrições do evento. A de voluntários manteve pendente/aprovado/reprovado; a de participantes não tem aprovação, porque ninguém aprova quem vai ao evento. Nas telas públicas, "Inscreva-se" passou a apontar para a inscrição de participante — antes abria o formulário de voluntariado — e cada botão só aparece quando o evento abriu aquele fluxo, no lugar do "Seja Voluntário" que era exibido sempre e não fazia nada. 16 testes E2E.<br>**Integrado com o back-end.** O front foi escrito antes de o back-end expor o fluxo e usava nomes supostos; alinhado ao contrato publicado — campo `formulario_participante_link` no evento e `GET /formulario/eventos/{id}/participantes` para a listagem, que devolve `participante_id` e exige o setor Inscrições. A troca custou três linhas, porque os nomes estavam isolados na camada de serviço. Não resta pendência nesta história. |
| RF53 | Cadastro nacional de líderes de jovens e adolescentes | A | US10 — Alta (2ª prioridade da cliente, 16/10/2026) | | |
| RF54 | Área de materiais para download | A | US11 — Alta (1ª prioridade da cliente, 16/10/2026) | | |

### Requisitos surgidos na reunião de validação (16/10/2026)

| ID | Requisito | Origem | Situação | Responsável | O que foi feito |
|---|---|---|---|---|---|
| RF55 | Excluir fotos específicas do álbum de um evento | R | US18 — Média | | |
| RF56 | Separar eventos online e presenciais | R | US19 — Média | | |
| RF57 | Gestão de pedidos de produto no setor Comercial | R | US21 — escopo pendente | | |

### Requisitos não funcionais

| ID | Requisito | Origem | Situação | Responsável | O que foi feito |
|---|---|---|---|---|---|
| RNF01 | Otimização de desempenho no celular | S | **Fora do escopo** — site classificado como "muito rápido" | | |
| RNF02 | Acessibilidade e leitor de tela | S | US16 — Baixa | [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206), [Renan Vieira Guedes](https://github.com/R-enanVieira) | **Parcial e incidental.** Rótulos acessíveis acrescentados onde as histórias passaram: navegação de mês do calendário, campos de dia do evento, busca de convidados e botões de remover. Não houve revisão de acessibilidade do site — isso segue sendo o escopo da US16. |
| RNF03 | Prévia correta ao compartilhar o link | S | US08 — Média | | |
| RNF04 | SEO — melhor posicionamento no Google | S | Backlog | | |
| RNF05 | Layout revisado em telas pequenas | S | Backlog | | |
| RNF06 | Mensagem de erro clara no lugar de tela branca | S | Backlog | | |
| RNF07 | Backup e restauração dos dados | S | Backlog | | |
| RNF08 | Padronização de fuso horário nas datas | T | Backlog | [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206), [Renan Vieira Guedes](https://github.com/R-enanVieira) | **Parcial, no front.** A leitura de data passou a usar o relógio de parede da string em vez de `new Date`, que interpretava data sem hora como UTC e mostrava um dia a menos. Verificado contra o back-end: a coluna é `timestamptz`, não há conversão no caminho, e a ida e volta é estável. |
| RNF09 | Funcionar bem com internet ruim | S | Backlog | | |
| RNF10 | Manual de uso do painel para a equipe | S | Backlog | | |

---

## Histórico de Versão

| Versão | Data | Descrição | Autor(es) |
|---|---|---|---|
| `1.0` | 31/08/2026 | Criação do backlog de melhorias a partir da elicitação com a cliente | [Júlia dos Reis Teixeira Massuda](https://github.com/JuliaReis18) |
| `1.1` | 31/08/2026 | Inclusão do catálogo completo de requisitos elicitados, com coluna de responsável por requisito | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) |
| `1.2` | 11/09/2026 | Inclusão do campo "O que foi feito" no catálogo e registro das entregas de QA da Sprint 1 | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) |
| `1.3` | 13/09/2026 | Registro da implementação dos requisitos das histórias US01, US03 e US06 no catálogo, ao lado das entregas de QA já cadastradas | [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr) |
| `1.4` | 13/09/2026 | Registro dos requisitos tocados de forma parcial pelas histórias US01, US03 e US06 — RF51, RNF02 e RNF08 | [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr) |
| `1.5` | 13/09/2026 | Registro das entregas de QA do front-end — correção do idioma, metadados de compartilhamento e testes de regressão da duplicação | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) e [Rivadalvio Joaquim da Silva Filho](https://github.com/RivaFilho)   |
| `1.6` | 13/09/2026 | Registro da implementação das histórias US02, US04 e US05 no catálogo — RF11, RF02, RF03, RF04 e RF06 | [Gabriel Lopes de Amorim](https://github.com/BrzGab), [Maria Samara Alves Silva](https://github.com/SamaraAlvess) e [João Vitor Alves Viana](https://github.com/Joaovitor045) |
| `1.7` | 14/09/2026 | Registro dos protótipos das histórias US02 e US05 (RF11, RF02 e RF03) feitos pelo trio | [Gabriel Lopes de Amorim](https://github.com/BrzGab), [Maria Samara Alves Silva](https://github.com/SamaraAlvess) e [João Vitor Alves Viana](https://github.com/Joaovitor045) |
| `1.8` | 14/09/2026 | Registro da implementação do back-end da US05 (RF02) — persistência dos campos de perfil do líder | [Filipe Carvalho da Silva](https://github.com/Filipe-002), [João Rodrigues](https://github.com/JpRodrigues2) e [Júlia dos Reis Teixeira Massuda](https://github.com/JuliaReis18) |
| `1.9` | 14/09/2026 | Registro da contribuição em dupla do back-end nas histórias US01 e US03 e ajuste da atribuição de RF05 e RF01 entre front-end e back-end | [Luiz Henrique Guimarães Soares](https://github.com/luizh-gsoares) e [Daniel dos Santos Barros de Sousa](https://github.com/daniel-de-sousa) |
| `2.0` | 15/09/2026 | Altera o registro dos nomes dos membros do grupo, afim de facilitar a busca | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin)|
| `2.1` | 21/09/2026 | Registro das entregas de QA do back-end da Sprint 2 — RF01, RF47 e RF49 | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) |
| `2.2` | 05/10/2026 |Complemento do Registro das entregas de QA do back-end da Sprint 2 — RF01, RF47 e RF49 | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) |
| `2.3` | 05/10/2026 | Registro da implementação do front-end da US07 (RF07 e RF23) e do envio das US04 e US05 para produção (RF02 e RF06) | [Gabriel Lopes de Amorim](https://github.com/BrzGab), [Maria Samara Alves Silva](https://github.com/SamaraAlvess) e [João Vitor Alves Viana](https://github.com/Joaovitor045) |
| `2.4` | 05/10/2026 | Registro da implementação da US09 (RF52) na Sprint 2, com responsáveis, status, a divisão em doze tasks e a integração entre front e back-end | [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [Renan Vieira Guedes](https://github.com/R-enanVieira) e [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206) |
| `2.5` | 05/10/2026 | Registro da implementação da US15 (RF13) na Sprint 2 — histórico de eventos passados com fotos | [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [Renan Vieira Guedes](https://github.com/R-enanVieira) e [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206) |
| `2.6` | 05/10/2026 | Remoção da tabela de doze tasks da US09, redundante com a descrição da entrega no RF52 e no relatório da sprint | [Davi Emanuel Ribeiro de Oliveira](https://github.com/daviRolvr), [Renan Vieira Guedes](https://github.com/R-enanVieira) e [João Pedro Ferreira Moraes](https://github.com/JoaoPedro2206) |
| `2.7` | 17/10/2026 | Incorporação da reunião de validação de 16/10/2026: aceite das US01, US04, US05, US07 e US09; nova ordem de prioridade (US11, US10, US14); revisão da US03; histórias novas US18 a US22; US17 refinada; RF55 a RF57; lista de dúvidas pendentes | [Rivadalvio Joaquim da Silva Filho](https://github.com/RivaFilho) |
