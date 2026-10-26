# Guia do Front-end — US10, US11 e US14

**Sprint:** 3 · **Back-end:** [PR #16](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Backend/pull/16)
**Responsáveis pelo back-end:** [Júlia dos Reis Teixeira Massuda](https://github.com/JuliaReis18), [Filipe Carvalho da Silva](https://github.com/Filipe-002) e [João Rodrigues](https://github.com/JpRodrigues2)

Este guia reúne o que o front-end precisa para usar o back-end das três prioridades da cliente. Todas as chamadas usam o `api` de `src/services/api.js`, que já envia o token do Keycloak. Os arquivos de exemplo podem ser copiados para `src/services/api/` sem mudanças.

## Resumo

| História | Onde aparece | Chamada | O que o front faz |
|---|---|---|---|
| [US10](backlog_melhorias.md#us10) | Página inicial do painel (só superadmin) | `GET /painel/links` · `PUT /painel/links` | Mostra os dois links fixados e permite trocá-los |
| [US10](backlog_melhorias.md#us10) | Cadastro e edição de líder | `POST /lider/` · `PUT /lider/{id}` | Trata o novo erro **409** (cargo ocupado) |
| [US11](backlog_melhorias.md#us11) | Área pública de materiais | `GET /material/` | Lista e mostra o botão "Baixar" |
| [US11](backlog_melhorias.md#us11) | Painel de materiais (só superadmin) | `POST`, `PUT` e `DELETE /material/` | Cadastra, edita e exclui |
| [US11](backlog_melhorias.md#us11) | Galerias de fotos | nenhuma chamada nova | Usa o `url_download` de cada foto |
| [US14](backlog_melhorias.md#us14) | Edição do evento no painel | `PATCH /evento/{id}/situacao` | Marca como cancelado ou adiado, ou desfaz |
| [US14](backlog_melhorias.md#us14) | Agenda, calendário, home e página do evento | nenhuma chamada nova | Mostra o aviso e esconde os botões de inscrição |

---

## US10 — Cadastro nacional de líderes

### Links fixados no painel

Os dois links ficam na página inicial do painel e só a superadministradora os vê. O back-end recusa com **403** qualquer outro papel, inclusive na leitura, porque a planilha tem dados pessoais de mais de 2500 líderes. Para o administrador de setor, basta não chamar a rota.

**`GET /painel/links`** devolve:

```json
{
  "link_cadastro_lideres": "https://forms.gle/...",
  "link_planilha_lideres": null
}
```

`null` quer dizer que o link ainda não foi configurado. Nesse caso, o card some ou mostra "Configurar".

**`PUT /painel/links`** faz atualização parcial: só os campos enviados mudam. Para remover um link, envie `""`. O link precisa começar com `https://` ou `http://`; se não começar, a resposta é **422**.

```json
{ "link_planilha_lideres": "https://docs.google.com/spreadsheets/d/..." }
```

A resposta é o mesmo objeto do GET, já atualizado.

```js
// src/services/api/painelApi.js
import { api } from "../api";

/** GET /painel/links (superadmin) */
export async function obterLinksPainel() {
  const { data } = await api.get("/painel/links");
  return data; // { link_cadastro_lideres, link_planilha_lideres }
}

/** PUT /painel/links (superadmin). Envie só o que mudou; "" remove o link. */
export async function atualizarLinksPainel(links) {
  const { data } = await api.put("/painel/links", links);
  return data;
}
```

### Cargo único

`POST /lider/` e `PUT /lider/{id}` agora respondem **409** quando já existe um líder ativo no mesmo cargo e na mesma região. A mensagem vem pronta em `detail` e pode ser exibida como está:

```json
{ "detail": "O cargo 'Diretor Regional' na região Sul já está ocupado por Ana Souza. Marque o líder atual como diretor anterior antes de cadastrar o novo." }
```

A regra é esta:

- O cargo é o par **cargo + região**. "Diretor Regional" do Sul e do Norte convivem.
- A comparação ignora acento, maiúsculas e hífen. "Vice-Diretor Nacional" e "vice diretor nacional" contam como o mesmo cargo.
- Líder marcado como **diretor anterior** (`is_antigo: true`) não ocupa cargo. Na troca de mandato, primeiro se marca o antigo como anterior e depois se cadastra o novo.

```js
try {
  await criarLider(dados);
} catch (erro) {
  if (erro.response?.status === 409) {
    mostrarErro(erro.response.data.detail);
    return;
  }
  throw erro;
}
```

---

## US11 — Materiais para download

### Área pública

**`GET /material/`** é público e não precisa de login. O filtro `?tipo=foto`, `?tipo=video` ou `?tipo=documento` é opcional. A lista vem do mais novo para o mais antigo.

```json
[
  {
    "material_id": 3,
    "titulo": "Manual do Líder 2026",
    "descricao": "Versão para impressão",
    "tipo": "documento",
    "link": "https://drive.google.com/file/d/1AbC.../view",
    "url_download": "https://<api>/drive/download/1AbC...",
    "criado_em": "2026-10-26T19:18:29Z"
  }
]
```

O botão "Baixar" é só um link para `url_download`:

```jsx
<a href={material.url_download} download>Baixar</a>
```

O que vem em `url_download`:

- **Arquivo do Drive:** o download passa pelo back-end, sai com o nome original e não pede login. Documento do Google (Docs, Planilhas, Apresentações) sai em PDF.
- **Link externo, como um vídeo no YouTube:** `url_download` é o próprio link. Nesse caso, faz mais sentido abrir em nova aba (`target="_blank"`).

### Painel (só superadmin)

| Ação | Chamada | Corpo |
|---|---|---|
| Cadastrar | `POST /material/` → 201 | `{ "titulo", "tipo", "link", "descricao"? }` |
| Editar | `PUT /material/{id}` → 200 | mesmo corpo |
| Excluir | `DELETE /material/{id}` → 204 | — |

- `tipo` aceita `"foto"`, `"video"` ou `"documento"`.
- `link` é o link de compartilhamento do Drive e precisa começar com `https://`.
- `titulo` não pode ficar em branco.
- Erro de validação responde **422**, administrador de setor recebe **403** e material inexistente responde **404**.

O arquivo do Drive precisa estar compartilhado com a conta institucional, a mesma da galeria.

```js
// src/services/api/materialApi.js
import { api } from "../api";

/** GET /material/ (publico). tipo: "foto" | "video" | "documento" | undefined */
export async function listarMateriais(tipo) {
  const { data } = await api.get("/material/", { params: tipo ? { tipo } : {} });
  return data;
}

/** POST /material/ (superadmin) */
export async function criarMaterial(material) {
  const { data } = await api.post("/material/", material);
  return data;
}

/** PUT /material/{id} (superadmin) */
export async function atualizarMaterial(id, material) {
  const { data } = await api.put(`/material/${id}`, material);
  return data;
}

/** DELETE /material/{id} (superadmin) */
export async function deletarMaterial(id) {
  await api.delete(`/material/${id}`);
}
```

### Download nas galerias

Cada foto de `GET /evento/{id}/galeria` e de `GET /evento/galerias/todas` passou a trazer `url_download`. Não há chamada nova: basta colocar o botão "Baixar" ao lado da foto, com o mesmo `<a href={foto.url_download} download>`.

---

## US14 — Evento cancelado ou adiado

### Campos novos no evento

Todo evento (`GET /evento/`, `GET /evento/{id}`, busca) traz dois campos novos:

| Campo | Valores | Observação |
|---|---|---|
| `situacao` | `"ativo"`, `"cancelado"`, `"adiado"` | Sempre presente. Eventos antigos vêm como `"ativo"` |
| `nova_data` | data e hora ISO, ou `null` | Só no adiado, e só quando a nova data já existe |

### Marcar, adiar e desfazer (painel)

**`PATCH /evento/{id}/situacao`** é liberada para o setor de eventos e para a superadmin. O corpo é pequeno, e não é preciso reenviar o evento inteiro.

```json
{ "situacao": "cancelado" }
{ "situacao": "adiado", "nova_data": "2027-04-10T09:00:00Z" }
{ "situacao": "adiado" }
{ "situacao": "ativo" }
```

O último exemplo desfaz a marcação. A resposta é o evento completo, já atualizado. Ao marcar, nada é apagado e ninguém é avisado; o título na agenda do Google ganha `[CANCELADO]` ou `[ADIADO]`.

```js
// em src/services/eventService.js (ou num eventoApi.js)
export async function alterarSituacaoEvento(id, situacao, novaData = null) {
  const { data } = await api.patch(`/evento/${id}/situacao`, {
    situacao,
    nova_data: novaData,
  });
  return data;
}
```

O `PUT /evento/{id}` do formulário completo também aceita `situacao` e `nova_data`. Se o formulário não enviar `situacao`, o adiamento é preservado.

### Exibição no site

Nas telas de agenda, calendário, home e página do evento, o comportamento é este:

```js
const cancelado = evento.situacao === "cancelado";
const adiado = evento.situacao === "adiado";
const mostrarInscricao = evento.situacao === "ativo"; // CA4: oculta os botões

// Aviso:
//   cancelado            -> "Evento cancelado"
//   adiado + nova_data   -> "Adiado para 10/04/2027"
//   adiado sem nova_data -> "Evento adiado — nova data em breve"
```

O evento cancelado ou adiado **continua aparecendo** nas listas. Não filtre esses eventos.

---

## Erros comuns

| Código | Quando |
|---|---|
| 401 | Token expirado. O interceptor do `api.js` já redireciona para o login |
| 403 | Papel sem permissão: links do painel e escrita de materiais são só da superadmin; a situação do evento é do setor de eventos |
| 404 | Material ou evento inexistente, ou download de arquivo que não é imagem nem material cadastrado |
| 409 | Cargo de líder já ocupado (US10) |
| 422 | Validação: link sem `https://`, tipo de material ou situação fora da lista, título vazio |
| 502 | Falha ao falar com o Google Drive no download |

---

## Histórico de Versão

| Versão | Data | Descrição | Autor(es) |
|---|---|---|---|
| `1.0` | 26/10/2026 | Criação do guia de integração do front-end com o back-end das US10, US11 e US14 | [Júlia dos Reis Teixeira Massuda](https://github.com/JuliaReis18), [Filipe Carvalho da Silva](https://github.com/Filipe-002) e [João Rodrigues](https://github.com/JpRodrigues2) |
