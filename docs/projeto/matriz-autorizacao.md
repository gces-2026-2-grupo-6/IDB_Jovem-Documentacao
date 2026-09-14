# Matriz de Autorização

**Sprint:** 1 · **Entrega:** QA05 · **História:** [US03](backlog_melhorias.md#us03)
**Responsável:** [João Pedro](https://github.com/Jadequilin)

---

## Objetivo

Documentar, rota a rota, qual papel é exigido para cada operação do back-end. A matriz serve a três propósitos: registrar o comportamento vigente **antes** da implementação da US03, expor as lacunas encontradas na auditoria, e servir de referência para o teste automatizado que impede regressão.

O arquivo `tests/unit/test_autorizacao.py`, no repositório do back-end espelha esta matriz. Se um controlador mudar a exigência de papel sem que este documento seja atualizado, o teste falha.

---

## Papéis existentes

O sistema reconhece apenas dois papéis de *realm* no Keycloak:

| Papel | Descrição |
|---|---|
| `admin` | Administrador geral. Acessa a maior parte das operações de conteúdo. |
| `superadmin` | Administrador máximo. Exclusivo para produtos e gestão de administradores. |

**Não existe segmentação por setor.** A US03 exige criar essa granularidade — segundo a elicitação, para **Loja/Produtos**, **Agenda/Eventos** e **Inscrições**, mantendo a exclusão restrita ao superadministrador e sem separação entre Jovem e Teen.

---

## Matriz atual

| Módulo | Rota | Método | `admin` | `superadmin` | Anônimo |
|---|---|---|---|---|---|
| Evento | `/evento` | POST | Sim | Sim | Não |
| Evento | `/evento` | GET | Sim | Sim | **Sim** |
| Evento | `/evento/{id}` | PUT | Sim | Sim | Não |
| Evento | `/evento/{id}` | DELETE | Sim | Sim | Não |
| Líder | `/lider` | POST | Sim | Sim | Não |
| Líder | `/lider` | GET | Sim | Sim | **Sim** |
| Líder | `/lider/{id}` | PUT | Sim | Sim | Não |
| Líder | `/lider/{id}` | DELETE | Sim | Sim | Não |
| Produto | `/produto` | POST | **Não** | Sim | Não |
| Produto | `/produto/{id}` | PUT | **Não** | Sim | Não |
| Produto | `/produto/{id}` | DELETE | **Não** | Sim | Não |
| Voluntário | `/voluntario/*` | POST/PUT/DELETE | Sim | Sim | Não |
| Banda/Palestrante | `/banda-palestrante` | POST/PUT/DELETE | Sim | Sim | Não |
| Admin | `/admin` | POST | **Não** | Sim | Não |
| Admin | `/admin` | GET | Sim | Sim | Não |
| Admin | `/admin/{id}` | DELETE | **Não** | Sim | Não |
| **Atividade** | `/evento/{id}/atividade` | POST | Sim | Sim | **Sim** |
| **Atividade** | `/evento/atividade/{id}` | PUT | Sim | Sim | **Sim** |
| **Atividade** | `/evento/atividade/{id}` | DELETE | Sim | Sim | **Sim** |

---

## Lacunas encontradas na auditoria

### 1. Rotas de atividade sem qualquer proteção

**Severidade: alta.**

O `src/atividade/controller.py` registra POST, PUT e DELETE **sem nenhuma dependência de verificação de papel**, e o router não é incluído com dependências em `src/main.py`. Na prática, qualquer pessoa com acesso à API pode criar, alterar ou apagar a programação de qualquer evento, sem autenticação.

O contraste chama atenção: o evento em si é protegido, mas a programação dele não. Provável descuido de implementação, já que todos os outros módulos de escrita aplicam `verificar_roles`.

**Correção sugerida:** aplicar `Depends(verificar_roles(["admin", "superadmin"]))` nas três rotas de escrita, mantendo as de leitura públicas — coerente com o padrão de evento e líder.

O teste `TestRotasSemProtecao` documenta a ausência. Quando a proteção for adicionada, ele falha propositalmente, sinalizando que as rotas devem migrar para a matriz principal.

### 2. Comparação de papéis vulnerável a payload malformado

**Severidade: média.**

A função `verificar_roles` fazia `role in roles_usuario`. Quando `roles_usuario` não é uma lista — por exemplo, uma string —, o operador `in` passa a comparar substrings. O efeito prático é que `"admin" in "superadmin"` retorna verdadeiro, concedendo acesso indevido.

Explorar isso exige um token assinado pelo Keycloak com o campo malformado, o que reduz a probabilidade. Ainda assim, é falha de programação defensiva numa função de autorização.

**Correção aplicada:** validação de tipo antes da comparação, tratando qualquer valor que não seja coleção como lista vazia. Coberto por `test_roles_como_string_nao_concede_acesso`.

### 3. Papel de cliente não substitui papel de realm

Comportamento correto, mas não testado até agora. Um token com `resource_access.jovem-backend.roles = ["superadmin"]` e `realm_access.roles = []` é corretamente negado. O teste fixa esse comportamento para que uma futura mudança em `verificar_roles` não o afrouxe por engano.

---

## Impacto na implementação da US03

A introdução de papéis por setor deve preservar as garantias já testadas:

- Exclusão permanece restrita ao superadministrador em todos os módulos.
- Papel desconhecido ou ausente continua resultando em 403.
- A comparação de papéis permanece sensível a maiúsculas e exige correspondência exata.
- Papéis de cliente seguem sem valer como papéis de realm.

A alteração ocorre em três camadas — configuração do Keycloak, guardas de rota no front-end e validação no back-end. Esta matriz cobre apenas a terceira. A verificação da camada de interface é entrega do QA-2.

---

## Histórico de Versão

| Versão | Data | Descrição | Autor(es) |
|---|---|---|---|
| `1.0` | 01/09/2026 | Criação da matriz e registro das lacunas da auditoria | [João Pedro](https://github.com/Jadequilin) |
