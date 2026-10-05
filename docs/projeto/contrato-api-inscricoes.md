# Contrato da API de Inscrições (US09)

**História:** [US09 — Inscrição separada para voluntários](backlog_melhorias.md#us09) · **Requisito:** RF52
**Repositório:** `IDB_Jovem-Backend`, branch `feat/us09`
**Situação:** back-end concluído · integração do front-end pendente

---

## Sumário

- [O que mudou](#mudou)
- [Autorização](#autorizacao)
- [Endpoints](#endpoints)
- [Campos do evento](#evento)
- [Resposta da listagem](#resposta)
- [Erros](#erros)
- [O que o front-end precisa fazer](#front)
- [Fora do escopo](#fora)
- [Como verificar localmente](#verificar)

---

<a name="mudou"></a>

## O que mudou

Até a Sprint 2, o evento tinha **um único** link de inscrição, usado para voluntariado. A US09 separou os dois públicos: quem vai **participar** do evento e quem vai **trabalhar** nele. Agora cada fluxo tem o seu link e a sua listagem.

| Fluxo | Link no evento | Listagem | Status de aprovação |
|---|---|---|---|
| Voluntariado (já existia) | `formulario_link` | `GET /formulario/eventos/{id}/inscricoes` | pendente / aprovado / reprovado |
| Participantes (novo) | `formulario_participante_link` | `GET /formulario/eventos/{id}/participantes` | **não tem** |

Os dois links são de **Google Forms**, e é daí que sai a listagem: o back-end lê as respostas do formulário e as cruza com o banco.

---

<a name="autorizacao"></a>

## Autorização

As duas listagens devolvem **nome e e-mail** de quem se inscreveu, então ambas exigem o setor de **Inscrições**:

| Quem | Acesso |
|---|---|
| `superadmin` | sim |
| `admin` com `admin-inscricoes` | sim |
| `admin` de outro setor | **403** |
| sem token | **401/403** |

O token é o JWT do Keycloak, em `Authorization: Bearer <token>`. Nada muda em relação ao que a listagem de voluntários já exigia desde a QA10.

---

<a name="endpoints"></a>

## Endpoints

| Método | Rota | Autorização | Sucesso | Erros |
|---|---|---|---|---|
| `GET` | `/formulario/eventos/{evento_id}/inscricoes` | setor Inscrições | `200` lista de voluntários | `404`, `502` |
| `GET` | `/formulario/eventos/{evento_id}/participantes` | setor Inscrições | `200` lista de participantes | `404`, `502` |

A rota de voluntários **não mudou**: o front que já a consome segue funcionando.

---

<a name="evento"></a>

## Campos do evento

`POST /evento/` e `PUT /evento/{id}` aceitam o campo novo, opcional:

```json
{
  "nome": "Acampamento de Verão",
  "formulario_link": "https://docs.google.com/forms/d/VOLUNTARIOS/edit",
  "formulario_participante_link": "https://docs.google.com/forms/d/PARTICIPANTES/edit"
}
```

| Campo | Significado |
|---|---|
| `formulario_link` | formulário de **voluntariado** (já existia; o sentido não mudou) |
| `formulario_participante_link` | formulário de **participantes** (novo, opcional) |

Um fluxo não supre a falta do outro: pedir a listagem de participantes num evento que só tem link de voluntário responde `404`.

---

<a name="resposta"></a>

## Resposta da listagem

`GET /formulario/eventos/1/participantes`

```json
[
  {
    "evento_id": 1,
    "participante_id": 7,
    "nome": "Ana Souza",
    "email": "ana@exemplo.org",
    "resposta_id": "resp-1",
    "link_resposta": "https://docs.google.com/forms/d/PARTICIPANTES/edit#response=resp-1"
  }
]
```

Repare na diferença para a listagem de voluntários: aqui **não existe `status`**. A aprovação pendente/aprovado/reprovado é do voluntariado, conforme o terceiro critério de aceitação da história.

Quem responde o formulário é gravado nas tabelas `participante` e `inscricao`. Uma resposta sem nome, sem e-mail ou sem identificador é descartada. Responder de novo com o mesmo e-mail atualiza o cadastro, não duplica.

---

<a name="erros"></a>

## Erros

| Código | Quando | Corpo |
|---|---|---|
| `404` | evento inexistente | `{"detail": "Evento nao encontrado"}` |
| `404` | evento sem link de participantes | `{"detail": "Evento sem formulario de participantes configurado"}` |
| `502` | falha ao falar com o Google Forms | `{"detail": "Falha ao buscar respostas no Google Forms"}` |
| `403` | token sem o setor de Inscrições | `{"detail": "Acesso negado ao setor 'inscricoes'..."}` |

---

<a name="front"></a>

## O que o front-end precisa fazer

**Checklist**

- [ ] Campo "link de inscrição de participantes" no formulário de evento, mapeado para `formulario_participante_link` em `eventService.toApiEvent`
- [ ] Ler o campo novo ao editar um evento
- [ ] Tela ou aba de listagem de participantes, consumindo a rota nova
- [ ] Não esperar `status` na listagem de participantes
- [ ] Botão público "Inscreva-se" apontando para o link de participantes, e "Seja Voluntário" para o de voluntariado

Hoje, na página de eventos, o botão **"Inscreva-se" abre o formulário de voluntários** (`linkFormularioVoluntarios`), porque era o único link existente. Com a US09, ele deve passar a usar o link de participantes.

---

<a name="fora"></a>

## Fora do escopo

- **Inscrição dentro da plataforma.** A inscrição continua acontecendo no Google Forms; o sistema lê as respostas.
- **Aprovação de participante.** Não há fluxo de aprovação: participante não tem status.
- **Controle de vagas e lista de espera** (RF16, no backlog).

---

<a name="verificar"></a>

## Como verificar localmente

```bash
docker compose up -d
docker compose exec jovem-backend alembic upgrade head     # aplica 4d7a1c3e8b52
docker compose exec jovem-backend pytest tests/unit/test_formulario_participante_repository.py \
  tests/unit/test_formulario_participante_service.py \
  tests/unit/test_formulario_participante_controller.py \
  tests/integration/test_formulario_participante.py
```

```bash
curl -H "Authorization: Bearer $TOKEN" \
  http://localhost:8000/formulario/eventos/1/participantes
```

---

## Histórico de Versão

| Versão | Data | Descrição | Autor(es) |
|---|---|---|---|
| `1.0` | 05/10/2026 | Criação do contrato da API de inscrições da US09 para a integração do front-end | [Filipe Carvalho da Silva](https://github.com/Filipe-002), [João Rodrigues](https://github.com/JpRodrigues2) e [Júlia dos Reis Teixeira Massuda](https://github.com/JuliaReis18) |
