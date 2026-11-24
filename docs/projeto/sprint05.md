# Sprint 05

*Período:* 20/11/2026 a 24/11/2026

Sprint sem trabalho planejado de antemão. O escopo foi reservado, de propósito,
para o que a cliente observasse usando o sistema — e foi exatamente o que
aconteceu: a reunião de validação de 23/11/2026 produziu a lista inteira desta
sprint.

## Entregas da Sprint

| Trio de Desenvolvedores | Qtd. de US | US | Assunto das US |
|---|---:|---|---|
| [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) / [Rivadalvio Joaquim da Silva Filho](https://github.com/RivaFilho) | 6 | US06, US09, US10, US11, US12 e US21 | Validação com a cliente tela a tela; diagnóstico e correção da integração com o Google Forms, que não trazia nenhuma inscrição; retirada da tela de Pedidos a pedido da cliente; calendário do painel cobrindo o dia inteiro e navegando por mês; botão público de cadastro de líderes; formatos aceitos na imagem de produto; lista de convidados que não espera mais a contagem de participações; matriz de autorização atualizada e guia de uso entregue à cliente |

> As demais linhas desta tabela são preenchidas por cada trio ao registrar sua
> entrega, como nas sprints anteriores.

---

## Validação com a cliente — 23/11/2026

Reunião com a Pra. Raquel Gomes, tela por tela, com o sistema em produção. A
maior parte foi aprovada sem ressalva. O que saiu virou código no mesmo dia.

Quatro achados e dois pedidos novos:

| # | O que apareceu | Tipo | Desfecho |
|---|---|---|---|
| 1 | Nenhuma inscrição do Google Forms chegava ao painel, e a tela mostrava lista vazia | Defeito | Corrigido |
| 2 | O calendário do painel não mostrava o evento das 20h | Defeito | Corrigido |
| 3 | A tela de Pedidos nunca teve o que listar | Escopo | Tela retirada |
| 4 | A foto do produto não aparecia na loja | Defeito | Corrigido |
| 5 | O link de cadastro de líderes deveria ficar na área pública | Requisito novo | Entregue |
| 6 | Os materiais deveriam mostrar uma prévia visual | Requisito novo | Entregue |

---

## US09 — A integração com o Google Forms não trazia nada

O achado mais sério da sprint, e o que custou mais tempo para diagnosticar.

### O sintoma

A cliente preenchia o próprio formulário de voluntários e a inscrição não
aparecia no painel. A tela mostrava **lista vazia, sem erro nenhum**.

### Três diagnósticos errados antes do certo

Vale o registro, porque o padrão se repete:

1. **O formato do link.** A tela aceitava `forms.gle/...` e `/forms/d/e/...`, que
   a API do Google não reconhece. Era defeito real — e foi corrigido —, mas
   **não era a causa**.
2. **Os títulos das perguntas.** A leitura exigia exatamente `Nome` e `Email`.
   Também era limitação real, também foi corrigida, também não era a causa.
3. **A credencial.** Suspeitou-se de *refresh token* vencido. Estava vivo e
   renovando normalmente.

A causa só apareceu quando se leu **a resposta da API**, em vez do código:

```
502 {"detail":"Falha ao buscar formulario no Google Forms"}
```

### A causa

**A conta Google do site (`idbjovemnacional@gmail.com`) não conseguia abrir o
formulário da cliente.** O formulário era dela, nunca foi compartilhado, e no
Google Forms **só editores enxergam as respostas** — leitor não basta.

Provado de ponta a ponta: um formulário criado na própria conta do site
respondeu 200 com as inscrições; um formulário pessoal compartilhado como
**Editor** também passou a funcionar.

### As duas regras que fazem a integração funcionar

1. O formulário precisa estar compartilhado com `idbjovemnacional@gmail.com`
   como **Editor**.
2. O link colado no painel precisa ser o de **edição**, terminado em `/edit`. O
   link de compartilhar (`/forms/d/e/.../viewform`) carrega outro identificador,
   que a API não aceita, e **não há como converter um no outro**.

Compartilhar com essa conta a pasta do Drive onde os formulários ficam resolve
de uma vez: o que nascer lá dentro já nasce compartilhado.

### O que foi corrigido no código

| Correção | Onde |
|---|---|
| O erro da integração passa a **aparecer na tela**, em vez de lista vazia muda | Front — `mensagemDeErro` extrai o `detail` da resposta |
| Cada formato de link errado ganha a **sua própria mensagem**, em vez de silêncio | Back — `src/formulario/leitura.py` |
| Os títulos de pergunta aceitos foram **ampliados** | Back — mesmo módulo |
| O campo do painel parou de sugerir `https://forms.gle/...`, que a API recusa | Front — `EventForm` |

**Títulos de pergunta aceitos.** A comparação ignora acento, maiúscula,
pontuação e o asterisco de obrigatória: `E-mail *` e `e mail` contam como a
mesma pergunta.

| Campo | Títulos reconhecidos |
|---|---|
| Nome | `Nome`, `Nome completo`, `Seu nome`, `Nome e sobrenome`, `Qual seu nome`, `Qual é o seu nome`, `Nome do participante`, `Nome do voluntário` |
| Email | `Email`, `E-mail`, `Seu email`, `Seu e-mail`, `Qual seu email`, `Qual é o seu email`, `Endereço de email`, `Email para contato`, `E-mail para contato` |

A lista é **fechada de propósito**. Casar por "contém" pegaria `Nome da igreja`
no formulário de líderes, que tem seis páginas de campos.

**Voluntário e participante continuam sendo dois formulários separados**, um por
campo no cadastro do evento. O site separa as listas **pelo campo onde o link
foi colado**, não por uma pergunta dentro do formulário — um formulário único
com "você é voluntário ou participante?" não funciona.

---

## US10 — O botão de cadastro na área pública

### O requisito novo

Os dois links da US10 ficavam fixados na home do painel (CA1 e CA2). A cliente
pediu que o **de cadastro** fosse para a **área pública**, porque é lá que os
pastores das regionais se cadastram sozinhos. A planilha de respostas continua
restrita.

Entregue com uma rota pública nova, que devolve **apenas** o formulário de
cadastro: `GET /painel/link-cadastro-lideres`. `GET /painel/links`, que devolve
os dois endereços, segue exigindo `superadmin` até para ler, porque um deles é a
planilha com nome, e-mail e WhatsApp de mais de 2500 pessoas.

### Um defeito que o CI não pegava

O botão foi escrito, mergeado e publicado — e **não aparecia no site**. Com o
link salvo, a rota pública respondendo e o build no ar.

A causa: o botão foi posto em `src/components/layout/Header/NavLinks.jsx`, um
arquivo que **nenhum outro importava**. O cabeçalho que os três layouts montam
de verdade é `Header/index.jsx`, que tem a própria cópia do nav e do menu do
celular. O componente com o botão nunca era renderizado.

Nenhum teste cobria o botão, então a suíte verde não dizia nada sobre ele.

**Correção:** o botão passou a viver em `Header/index.jsx`, no nav de desktop e
no menu do celular, e `NavLinks.jsx` foi removido — manter duas cópias do mesmo
cabeçalho foi o que produziu o defeito. Cinco testes novos olham a **página
pronta**, não o componente, justamente para que a próxima cópia esquecida
apareça como falha. Verificado: com o cabeçalho revertido, três dos cinco falham.

---

## US06 — A lista de convidados e o botão Excluir

### O sintoma

`test_excluir_convidado_vinculado_nao_quebra_a_tela`, no Selenium, estourava
`TimeoutException` **procurando o botão Excluir** em `/admin/palestrantes`.
Parecia intermitente: passava em ambiente recém-subido e falhava depois.

### A causa

A tela esperava as duas buscas juntas:

```js
const [lista, contagem] = await Promise.all([
  fetchSpeakers(),
  fetchParticipacoesPorConvidado(),
]);
```

`fetchParticipacoesPorConvidado` faz **uma requisição por evento**. A suíte cria
um evento por execução, no mesmo ambiente, então o número só cresce; o navegador
ainda serializa em lotes de seis.

Enquanto isso, **nada da lista existia no DOM** — nem o botão. Ele não estava
quebrado: ainda não tinha nascido quando o Selenium desistia. Não era
intermitência, era volume de dados.

Não há `xfail` nesse teste. A suposição anterior de que ele estava mascarado era
falsa.

### A correção

A lista renderiza assim que `fetchSpeakers` responde; a contagem entra depois,
por fora. Enquanto não sabe, a coluna mostra `...` em vez de afirmar
"Ainda não vinculado", que seria afirmação sobre dado que ainda não se tem.

Não é só problema de teste: cada evento novo deixava a tela mais lenta para a
cliente, para sempre.

---

## US12 — Os formatos aceitos na imagem de produto

Dois problemas na mesma tela:

- a nota do campo falava só de compartilhamento, nada de formato — quem falava
  de formato era o formulário de materiais;
- o preview escondia a imagem no `onError`. Um `.heic` de iPhone, que é o caso
  comum, **sumia sem dizer nada**, e a pessoa só descobria olhando a loja
  pública depois.

É o mesmo padrão que escondeu as inscrições: a falha existia, a tela não contava.

**Correção:** a nota passa a listar `.jpg`, `.png` e `.webp` e a avisar que
`.heic` não aparece no navegador; quando a imagem não carrega, entra um aviso
visível dizendo o que conferir.

---

## US21 — A tela de Pedidos saiu do painel

A cliente observou que a tela nunca teve o que listar: a loja pública **nunca
ofereceu o formulário de pedido ao visitante**, então nenhum pedido jamais foi
registrado.

Menu e rota removidos do painel. Os arquivos em `pages/Admin/Pedidos` e **todo o
back-end de `/pedido/` foram preservados de propósito**, com as mesmas
exigências de autorização registradas na Sprint 4 — é código morto, não código
apagado, e a entrega do trio que o construiu continua íntegra.

---

## Calendário do painel

O evento das 20h às 21h da cliente não aparecia em lugar nenhum. A grade
desenhava apenas das **8h às 17h**: o segmento recebia a hora de início, o laço
só percorria 8 a 17, nenhuma linha casava e nada era renderizado — sem erro e
sem aviso.

Para um ministério de jovens isso escondia quase tudo, porque culto, ensaio e
reunião são à noite.

A grade passou a cobrir o **dia inteiro**, com rolagem, e ganhou seletor de mês e
ano. Qualquer faixa menor que o dia inteiro só adiaria o mesmo defeito para o
próximo horário de fora.

Um segundo defeito foi corrigido no caminho: em eventos de "dias avulsos", os
dias do meio ignoravam o horário informado e ocupavam a coluna inteira.

---

## Documentação e entrega à cliente

**Matriz de Autorização atualizada.** A seção "Papéis existentes" era o retrato
da Sprint 1 e ainda afirmava que o Keycloak reconhecia apenas `admin` e
`superadmin` e que **não existia separação por setor** — o oposto da realidade
desde a US03. Quem consultasse tirava conclusão errada justamente sobre a
pergunta que a matriz existe para responder.

Ficou registrada também a **porta de retrocompatibilidade**: um `admin` sem papel
de setor alcança **todos** os setores, de propósito, para não trancar do lado de
fora quem já era administrador antes da US03. A consequência é contraintuitiva e
não tem aviso na interface — esquecer o papel de setor concede **mais** acesso,
não menos.

**Guia de uso entregue à cliente.** Documento de 10 páginas, com as telas do
próprio site e do Google Forms, cobrindo o que foi entregue, como abrir as
inscrições de voluntários e participantes, os títulos de pergunta aceitos e os
formatos de imagem. Não faz parte do site e não é publicado.

---

## Pendências

| Item | Estado |
|---|---|
| Rotação do *refresh token* do Google | **Em aberto.** O token passou por logs na Sprint 4 e está comprometido. Confirmado em 23/11 que segue vivo e funcionando — ou seja, quem o tiver tem o mesmo acesso que o site. Adiado conscientemente para não travar a entrega |
| Sincronização automática das inscrições | A leitura do Forms acontece quando alguém abre a tela do painel, não sozinha |

---

## Histórico de Versão

| Versão | Data | Descrição | Autor(es) |
|---|---|---|---|
| `1.0` | 24/11/2026 | Criação do documento: validação com a cliente em 23/11, diagnóstico e correção da integração com o Google Forms, US10 na área pública, US06, US12, US21, calendário do painel, matriz de autorização e guia da cliente | [João Pedro Araújo de Freitas Lyra](https://github.com/Jadequilin) |
