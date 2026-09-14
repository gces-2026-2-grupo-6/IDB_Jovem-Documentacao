# Contrato da API de Líderes (US05)

**História:** [US05 — CRUD de líderes e diretores anteriores](backlog_melhorias.md#us05) · **Requisitos:** RF02, RF03
**Repositório:** `IDB_Jovem-Backend`, branch `feat/us05`
**Situação:** back-end concluído · integração do front-end pendente

---

## Sumário

- [Resumo para o front-end](#resumo)
- [Autorização](#autorizacao)
- [Endpoints](#endpoints)
- [Modelo de resposta](#resposta)
- [Criação e edição](#payload)
- [Regras de validação](#validacao)
- [Erros padronizados](#erros)
- [O que muda no front-end](#front)
- [Compatibilidade enquanto o front não for ajustado](#compatibilidade)
- [Fora do escopo](#fora)
- [Como verificar localmente](#verificar)

---

<a name="resumo"></a>

## Resumo para o front-end

A US05 foi dividida em três frentes. A **camada de dados** criou as colunas `regiao`, `mini_biografia` e `redes_sociais` na tabela `lider`. A **API** passou a aceitar e devolver esses campos, restringiu a escrita à superadministradora e ganhou duas listagens por situação. A **qualidade** cobriu o módulo com testes automatizados e registrou este contrato.

Para integrar, o front-end precisa de três ajustes, detalhados em [O que muda no front-end](#front):

1. Trocar `bio` por **`mini_biografia`**.
2. Enviar e ler **`redes_sociais` como objeto** `{"rede": "link"}`, não como texto.
3. Deixar de enviar `gestao`, que **não é persistido**.

---

<a name="autorizacao"></a>

## Autorização

| Operação | Papel exigido |
|---|---|
| Leitura (`GET`) | Nenhum — rotas públicas, usadas pela página inicial |
| Criação, edição e exclusão (`POST`, `PUT`, `DELETE`) | **`superadmin`** |

O token é o JWT do Keycloak, enviado em `Authorization: Bearer <token>`. O papel é lido de `realm_access.roles`.

- `admin` **não** escreve em líderes — recebe `403`.
- Papéis de setor da US03 (`admin-eventos`, `admin-produtos`, `admin-inscricoes`) **não** concedem acesso: líderes não são setor com administrador próprio.
- Não há papel novo a criar no Keycloak; `superadmin` já existe no realm.

Isso coincide com o que a tela "Diretores & Líderes" já faz no painel, restrita à superadministradora.

---

<a name="endpoints"></a>

## Endpoints

| Método | Rota | Autorização | Sucesso | Erros |
|---|---|---|---|---|
| `GET` | `/lider/` | pública | `200` lista | — |
| `GET` | `/lider/atuais` | pública | `200` lista | — |
| `GET` | `/lider/diretores-anteriores` | pública | `200` lista | — |
| `GET` | `/lider/{lider_id}` | pública | `200` objeto | `404`, `422` |
| `POST` | `/lider/` | `superadmin` | `201` objeto | `401`, `403`, `422` |
| `PUT` | `/lider/{lider_id}` | `superadmin` | `200` objeto | `401`, `403`, `404`, `422` |
| `DELETE` | `/lider/{lider_id}` | `superadmin` | `204` sem corpo | `401`, `403`, `404` |

- **Novas:** `/lider/atuais` devolve quem tem `is_antigo = false`; `/lider/diretores-anteriores`, quem tem `is_antigo = true`.
- **Ordenação:** todas as listagens vêm ordenadas por `ordem` e, em empate, por `lider_id`.
- `GET /lider/` continua devolvendo todos, como antes — o front atual segue funcionando sem trocar de rota.
- O contrato também está no Swagger do back-end, em `/docs`.

---

<a name="resposta"></a>

## Modelo de resposta

```json
{
  "lider_id": 1,
  "nome": "Ana Souza",
  "cargo": "Coordenadora Geral",
  "imagem_url": "https://drive.google.com/file/d/abc123/view",
  "is_antigo": false,
  "ordem": 1,
  "regiao": "Sudeste",
  "mini_biografia": "Atua na coordenação geral do ministério jovem desde 2022.",
  "redes_sociais": {
    "instagram": "https://instagram.com/exemplo",
    "linkedin": "https://linkedin.com/in/exemplo"
  }
}
```

| Campo | Tipo | Observação |
|---|---|---|
| `lider_id` | inteiro | identificador |
| `nome` | texto | obrigatório |
| `cargo` | texto | obrigatório |
| `imagem_url` | texto ou `null` | devolvido como foi salvo; a conversão do link do Drive continua no front |
| `is_antigo` | booleano | marcação **manual** de diretor anterior |
| `ordem` | inteiro | ordem de exibição |
| `regiao` | texto ou `null` | **novo** |
| `mini_biografia` | texto ou `null` | **novo** |
| `redes_sociais` | objeto ou `null` | **novo** — chave é o nome da rede, valor é o link |

---

<a name="payload"></a>

## Criação e edição

`POST` e `PUT` recebem o mesmo corpo. `nome` e `cargo` são obrigatórios; o restante é opcional.

```json
{
  "nome": "Ana Souza",
  "cargo": "Coordenadora Geral",
  "imagem_url": "https://drive.google.com/file/d/abc123/view",
  "is_antigo": false,
  "ordem": 1,
  "regiao": "Sudeste",
  "mini_biografia": "Atua na coordenação geral do ministério jovem desde 2022.",
  "redes_sociais": {
    "instagram": "https://instagram.com/exemplo"
  }
}
```

**Edição parcial.** No `PUT`, campo **omitido** mantém o valor atual; campo enviado como `null` (ou texto vazio, ver abaixo) é **apagado**. Na prática: para limpar as redes sociais, envie `"redes_sociais": null`; para não mexer nelas, não envie a chave.

**`is_antigo` não muda sozinho.** Nenhuma regra do sistema move um líder para "diretor anterior". Se o `PUT` não trouxer `is_antigo`, a marcação atual é preservada.

---

<a name="validacao"></a>

## Regras de validação

| Situação | Resultado |
|---|---|
| `regiao` ou `mini_biografia` vazios ou só com espaços | salvos como `null` |
| Espaços no início e no fim de `regiao`, `mini_biografia` e das redes | removidos |
| `redes_sociais` ausente, `null`, `""` ou `{}` | salvo como `null` |
| Rede com link vazio ou `null`, ex.: `{"linkedin": ""}` | a rede é descartada |
| `redes_sociais` em texto, ex.: `"@ana, youtube.com/ana"` | **`422`** |
| `redes_sociais` em lista ou número | **`422`** |
| Link de rede que não é texto, ex.: `{"instagram": 42}` | **`422`** |
| Nome de rede vazio, ex.: `{" ": "https://..."}` | **`422`** |
| Campos desconhecidos, ex.: `bio`, `gestao` | **ignorados**, sem erro |

O nome da rede é livre (`instagram`, `youtube`, `linkedin`, `tiktok`…) e o formato do link não é validado. Sugestão para o painel: oferecer uma lista fixa de redes, o que evita chaves como `Instagram` e `instagram` convivendo.

---

<a name="erros"></a>

## Erros padronizados

Todos os erros de regra e de autorização seguem o formato `{"detail": "<mensagem>"}`, declarado no OpenAPI como `ErroResposta`. O `422` segue o formato padrão de validação do FastAPI.

| Código | Quando | Corpo |
|---|---|---|
| `401` | token expirado | `{"detail": "O token de acesso expirou."}` |
| `401` | token inválido ou com assinatura incorreta | `{"detail": "Token invalido ou assinatura incorreta."}` |
| `403` | requisição sem token | `{"detail": "Not authenticated"}` |
| `403` | token sem o papel `superadmin` | `{"detail": "Acesso negado. Requer um dos papeis: superadmin"}` |
| `404` | líder inexistente | `{"detail": "Líder não encontrado."}` |
| `422` | corpo inválido | lista em `detail` — ver exemplo |

> Os `401` pressupõem o back-end com as variáveis `KEYCLOAK_*` configuradas. Sem elas, qualquer token enviado resulta em `500` com `{"detail": "Configuracao do Keycloak incompleta."}` — comportamento do módulo de segurança compartilhado, anterior à US05.

Exemplo de `422`:

```json
{
  "detail": [
    {
      "type": "value_error",
      "loc": ["body", "redes_sociais"],
      "msg": "Value error, redes_sociais deve ser um objeto JSON, ex.: {\"instagram\": \"https://instagram.com/perfil\"}"
    }
  ]
}
```

> **Atenção ao tratar erros no front.** No `422`, `detail` é uma **lista**, não um texto. O `resolveError` atual do `liderService.js` devolve `err.response.data.detail` diretamente, então precisa extrair `detail[0].msg` quando receber uma lista. Requisição **sem token** responde `403`, e não `401` — o interceptor do `api.js`, que só trata `401`, não redireciona para o login nesse caso.

---

<a name="front"></a>

## O que muda no front-end

Os ajustes se concentram em `src/services/liderService.js`, com reflexo no `LeaderForm`.

| Hoje no front | Novo contrato | Ajuste |
|---|---|---|
| lê e envia `bio` | `mini_biografia` | renomear no `toLeader` e no `toLiderPayload` |
| `redes_sociais` como texto separado por vírgula | objeto `{"rede": "link"}` | trocar o campo único por um campo por rede, montando o objeto |
| envia `gestao` | não existe | parar de enviar e remover da exibição, ou manter apenas visual sem persistência |
| envia `regiao` | `regiao` | nenhum |
| envia `is_antigo`, `ordem`, `imagem_url` | iguais | nenhum |
| usa só `GET /lider/` e separa no cliente | `GET /lider/atuais` e `/lider/diretores-anteriores` disponíveis | opcional |

Sugestão de mapeamento, como ponto de partida:

```js
function toLeader(api) {
  return {
    // ...campos atuais
    bio: api.mini_biografia ?? "",
    socialLinks: api.redes_sociais ?? {},          // objeto, não texto
  };
}

function toLiderPayload(form) {
  const redes = Object.fromEntries(
    Object.entries(form.socialLinks || {}).filter(([, link]) => link?.trim())
  );
  return {
    // ...campos atuais, sem `bio` e sem `gestao`
    mini_biografia: form.bio?.trim() || null,
    redes_sociais: Object.keys(redes).length ? redes : null,
  };
}
```

**Checklist de integração**

- [ ] `bio` → `mini_biografia` na leitura e no envio
- [ ] campo de redes sociais produz objeto; leitura trata objeto
- [ ] `gestao` removido do payload
- [ ] `resolveError` trata `detail` em lista (`422`)
- [ ] mocks de `tests/helpers/apiMock.js` e `mockApi.js` atualizados para o novo formato
- [ ] testes E2E de `tests/admin/lideres.spec.js` ajustados
- [ ] verificado contra o back-end real: criar, editar, limpar redes, excluir

---

<a name="compatibilidade"></a>

## Compatibilidade enquanto o front não for ajustado

O back-end foi mantido tolerante ao payload atual do painel, para que o merge não quebre a tela existente. O comportamento com o front **sem alterações** é este:

| Ação no painel atual | Resultado |
|---|---|
| Criar ou editar líder sem preencher redes sociais | funciona — `""` é tratado como ausência |
| Preencher região | funciona e passa a ser salva |
| Preencher mini-biografia | **não é salva** — o front envia `bio`, que é ignorado |
| Preencher redes sociais | **falha com `422`** — o front envia texto |
| Preencher período de gestão | **não é salvo** — `gestao` não existe |
| Editar líder cujas redes já estão salvas como objeto (ex.: via API) | **falha no próprio front** — o formulário trata o valor como texto e chama `.trim()` num objeto |
| Página inicial e galeria de diretores | funcionam — seguem usando `GET /lider/` |

O último caso só acontece se algum líder tiver redes cadastradas por outro meio antes do ajuste do front. Até lá, recomenda-se não cadastrar redes pela API diretamente.

---

<a name="fora"></a>

## Fora do escopo

- **Período de gestão (`gestao`).** Exibido hoje no card de diretores anteriores, mas não consta dos critérios de aceite da US05 nem da camada de dados entregue. Não é persistido.
- **Filtro "apenas cargo nacional" da galeria.** Continua no front (`splitLeaders`). A rota `/lider/diretores-anteriores` devolve todos os marcados como anteriores, independentemente da região.
- **Validação do formato dos links** das redes sociais.

---

<a name="verificar"></a>

## Como verificar localmente

No repositório do back-end, na branch `feat/us05`:

```bash
docker compose up -d
docker compose exec jovem-backend alembic upgrade head        # aplica 26e93df0f2ac
docker compose exec jovem-backend pytest tests/unit/test_lider_schema.py \
  tests/unit/test_lider_service.py tests/unit/test_lider_controller.py \
  tests/unit/test_lider_repository.py tests/integration/test_lider.py
```

Chamadas de exemplo (o token deve pertencer a um usuário com papel `superadmin`):

```bash
curl http://localhost:8000/lider/atuais

curl -X POST http://localhost:8000/lider/ \
  -H "Authorization: Bearer $TOKEN" -H "Content-Type: application/json" \
  -d '{"nome":"Ana Souza","cargo":"Coordenadora Geral","regiao":"Sudeste",
       "mini_biografia":"Bio curta.","redes_sociais":{"instagram":"https://instagram.com/exemplo"}}'
```

---

## Histórico de Versão

| Versão | Data | Descrição | Autor(es) |
|---|---|---|---|
| `1.0` | 14/09/2026 | Criação do contrato da API de líderes da US05 para a integração do front-end | [João Pedro Rodrigues](https://github.com/JpRodrigues2) |
