# Matriz de Autorização

**Sprint:** 1 · **Entrega:** QA05 · **História:** [US03](backlog_melhorias.md#us03)
**Responsável:** [João Pedro](https://github.com/Jadequilin)

---

## Objetivo

Documentar, rota a rota, qual papel é exigido para cada operação do back-end. A matriz serve a três propósitos: registrar o comportamento vigente **antes** da implementação da US03, expor as lacunas encontradas na auditoria, e servir de referência para o teste automatizado que impede regressão.

O arquivo `tests/unit/test_autorizacao.py`, no repositório do back-end espelha esta matriz. Se um controlador mudar a exigência de papel sem que este documento seja atualizado, o teste falha.

---

## Papéis existentes

> **Atualizado em 24/11/2026.** O texto anterior desta seção era o retrato da
> Sprint 1 e afirmava que não existia separação por setor. Isso deixou de ser
> verdade quando a US03 foi implementada, e quem consultasse o documento tirava
> conclusão errada sobre quem enxerga o quê.

O Keycloak reconhece cinco papéis de *realm*:

| Papel | Descrição |
|---|---|
| `superadmin` | Administrador máximo. Único que exclui conteúdo e gere administradores. |
| `admin` | Administrador. Sozinho não delimita nada — ver a ressalva abaixo. |
| `admin-eventos` | Setor de agenda: eventos, programação e convidados. |
| `admin-produtos` | Setor da loja: produtos. |
| `admin-inscricoes` | Setor de inscrições: voluntários e participantes. |

Toda conta de setor precisa de **dois** papéis: `admin` mais o do setor.

| Papéis da conta | O que a pessoa alcança |
|---|---|
| `admin` + `admin-eventos` | Eventos, Convidados |
| `admin` + `admin-inscricoes` | Voluntários |
| `admin` + `admin-produtos` | Produtos |
| `superadmin` | Tudo, incluindo Diretores & Líderes, Materiais, Missão e valores |
| `admin` **sem** papel de setor | **Todos os setores** |

### A porta de retrocompatibilidade

A última linha não é descuido. Um `admin` sem nenhum papel de setor alcança
**todos** os setores, de propósito: é o que impede que as contas criadas antes
da US03 fiquem trancadas do lado de fora no dia em que a separação entrou no ar.

A consequência prática é que **esquecer o papel de setor concede mais acesso, não
menos** — o oposto do que a intuição espera — e a interface não avisa nada. Ao
criar uma conta nova, conferir os dois papéis no Keycloak é obrigatório.

A regra mora em `src/utils/permissoes.js` no front e em `src/security.py` no
back; a tela não recalcula nada a partir do token.

---

## Matriz da Sprint 1 — retrato anterior à US03

A tabela abaixo é histórica: registra o comportamento **antes** da US03, que é o
que esta matriz nasceu para documentar. As exigências vigentes por setor estão
nas seções das sprints seguintes.

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

<a name="sprint3"></a>

## Rotas novas da Sprint 3 — US10, US11 e US14

Registradas na `MATRIZ` de `tests/unit/test_matriz_rotas.py`, que lê as guardas
das próprias rotas da aplicação ([PR #16](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Backend/pull/16)).

| Módulo | Rota | Método | Exigência | Motivo |
|---|---|---|---|---|
| Painel | `/painel/links` | GET | `superadmin` | US10: a planilha tem dados pessoais de mais de 2500 líderes, então até a leitura é restrita |
| Painel | `/painel/links` | PUT | `superadmin` | US10: só a superadministradora troca os links fixados |
| Material | `/material/` | GET | Pública | US11: "todas as pessoas podem baixar, sem login" |
| Material | `/material/{material_id}` | GET | Pública | US11 |
| Material | `/material/` | POST | `superadmin` | US11: "só o superadmin envia" |
| Material | `/material/{material_id}` | PUT | `superadmin` | US11 |
| Material | `/material/{material_id}` | DELETE | `superadmin` | Toda exclusão exige superadmin |
| Drive | `/drive/download/{file_id}` | GET | Pública | US11: download aberto. A rota só entrega imagem ou arquivo cadastrado como material; o resto responde 404 |
| Evento | `/evento/{evento_id}/situacao` | PATCH | Setor de eventos | US14: cancelar, adiar ou desfazer é edição do evento, não exclusão |

---

<a name="sprint4"></a>

## Rotas novas da Sprint 4 — US21

Registradas na `MATRIZ` de `tests/unit/test_matriz_rotas.py` e cobertas por
testes de acesso permitido e negado em `tests/unit/test_autorizacao_setores.py`
([PR #21](https://github.com/gces-2026-2-grupo-6/IDB_Jovem-Backend/pull/21)).

| Módulo | Rota | Método | Exigência | Motivo |
|---|---|---|---|---|
| Pedido | `/pedido/` | GET | Setor de produtos | US21 (CA1): a lista traz nome e contato de quem pediu |
| Pedido | `/pedido/{pedido_id}/status` | PATCH | Setor de produtos | US21 (CA2): mudar a situação é acompanhamento, não exclusão |
| Formulário | `/formulario/produtos/{produto_id}/pedidos` | GET | Setor de produtos | US21: sincroniza as respostas do formulário do produto e grava os pedidos |

O "setor de produtos" libera `admin-produtos` e `superadmin`. A US21 pede o
setor **Comercial**, mas o papel não existe no Keycloak, porque a revisão da
US03 (CA2 e CA3) ainda não foi implementada. As rotas reaproveitam o setor
atual em vez de criar um papel novo. A exclusão de produto continua exigindo
`superadmin` e passou a responder 409 quando o produto tem pedidos.

---

<a name="sprint5"></a>

## Rotas novas da Sprint 5 — US10 e US11

| Módulo | Rota | Método | Exigência | Motivo |
|---|---|---|---|---|
| Painel | `/painel/link-cadastro-lideres` | GET | Pública | US10: os pastores das regionais se cadastram sozinhos pelo site, então o endereço do formulário externo precisa ser legível sem login. Devolve **apenas** o link de cadastro — a planilha com os dados dos mais de 2500 líderes continua restrita em `/painel/links` |
| Drive | `/drive/miniatura/{file_id}` | GET | Pública | US11: a prévia visual aparece na área pública de materiais, junto do arquivo que já é de download aberto |

A separação entre as duas rotas de painel é o ponto: `/painel/links` devolve os
dois endereços e exige `superadmin` até para ler, porque um deles é a planilha
de respostas. `/painel/link-cadastro-lideres` devolve só o formulário de
preenchimento, que é público por natureza — é o que se divulga.

### Rota retirada

A tela de Pedidos (US21) saiu do painel em 23/11/2026, a pedido da cliente: a
loja pública nunca ofereceu o formulário de pedido ao visitante, então a tela
nunca teve o que listar. **As rotas de `/pedido/` continuam no back-end com as
mesmas exigências** registradas na Sprint 4; apenas deixaram de ser consumidas
pela interface.

---

## Histórico de Versão

| Versão | Data | Descrição | Autor(es) |
|---|---|---|---|
| `1.0` | 01/09/2026 | Criação da matriz e registro das lacunas da auditoria | [João Pedro](https://github.com/Jadequilin) |
| `1.1` | 26/10/2026 | Registro das rotas novas da Sprint 3 (US10, US11 e US14) | [Júlia dos Reis Teixeira Massuda](https://github.com/JuliaReis18), [Filipe Carvalho da Silva](https://github.com/Filipe-002) e [João Rodrigues](https://github.com/JpRodrigues2) |
| `1.2` | 17/11/2026 | Registro das rotas novas da Sprint 4 (US21) | [Júlia dos Reis Teixeira Massuda](https://github.com/JuliaReis18), [Filipe Carvalho da Silva](https://github.com/Filipe-002) e [João Rodrigues](https://github.com/JpRodrigues2) |
| `1.3` | 24/11/2026 | Papéis atualizados para o estado pós-US03, com a porta de retrocompatibilidade do `admin` sem setor; rotas da Sprint 5 (US10 e US11) e retirada da tela de Pedidos | [João Pedro](https://github.com/Jadequilin) |
