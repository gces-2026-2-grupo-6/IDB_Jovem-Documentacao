# Sprints

## Sumário

- [Sobre as Sprints](#sobre)
- [Equipe Responsável](#equipe)
- [Sprint 1](#sprint1)
- [Como Preencher Esta Página](#preencher)

---

<a name="sobre"></a>

## Sobre as Sprints

A partir de 09/2026, o software segue em produção e passa por um ciclo de manutenção e evolução organizado em **sprints de 2 semanas (14 dias)**. As demandas de cada sprint são retiradas do [Backlog de Melhorias](backlog_melhorias.md), priorizado junto à cliente por meio de entrevista e de um formulário de elicitação.

---

<a name="equipe"></a>

## Equipe Responsável

<center>

<table border="1" cellspacing="0" cellpadding="4">
  <thead>
    <tr>
      <th>Nome</th>
      <th>GitHub</th>
    </tr>
  </thead>
  <tbody>
    <tr><td>Gabriel Lopes</td><td><a href="https://github.com/BrzGab">@BrzGab</a></td></tr>
    <tr><td>Daniel Sousa</td><td><a href="https://github.com/daniel-de-sousa">@daniel-de-sousa</a></td></tr>
    <tr><td>Davi Emanuel Ribeiro de Oliveira</td><td><a href="https://github.com/daviRolvr">@daviRolvr</a></td></tr>
    <tr><td>Carlos Henrique</td><td><a href="https://github.com/Depaiiva">@Depaiiva</a></td></tr>
    <tr><td>Filipe</td><td><a href="https://github.com/Filipe-002">@Filipe-002</a></td></tr>
    <tr><td>João Pedro</td><td><a href="https://github.com/Jadequilin">@Jadequilin</a></td></tr>
    <tr><td>João Vitor Alves Viana</td><td><a href="https://github.com/Joaovitor045">@Joaovitor045</a></td></tr>
    <tr><td>João Pedro Rodrigues</td><td><a href="https://github.com/JpRodrigues2">@JpRodrigues2</a></td></tr>
    <tr><td>Júlia Massuda</td><td><a href="https://github.com/JuliaReis18">@JuliaReis18</a></td></tr>
    <tr><td>Luiz Henrique Soares</td><td><a href="https://github.com/luizh-gsoares">@luizh-gsoares</a></td></tr>
    <tr><td>Renan Vieira</td><td><a href="https://github.com/R-enanVieira">@R-enanVieira</a></td></tr>
    <tr><td>Riva Filho</td><td><a href="https://github.com/RivaFilho">@RivaFilho</a></td></tr>
    <tr><td>Samara Alves</td><td><a href="https://github.com/SamaraAlvess">@SamaraAlvess</a></td></tr>
  </tbody>
</table>

</center>

> Nomes obtidos a partir do perfil público do GitHub de cada integrante. Corrija diretamente nesta tabela caso algum nome esteja incompleto ou incorreto.

---

<a name="sprint1"></a>

## Sprint 1

<table border="1" cellspacing="0" cellpadding="4">
<tbody>
<tr><td><strong>Início</strong></td><td>01/09/2026</td></tr>
<tr><td><strong>Término</strong></td><td>14/09/2026</td></tr>
<tr><td><strong>Duração</strong></td><td>2 semanas (14 dias)</td></tr>
</tbody>
</table>

Todas as histórias classificadas como **Prioridade Alta** no Backlog de Melhorias entraram no escopo desta sprint.

<center>

<table border="1" cellspacing="0" cellpadding="4">
  <thead>
    <tr>
      <th>ID</th>
      <th>Tema</th>
      <th>História (resumo)</th>
      <th>Requisitos</th>
      <th>Responsáveis</th>
      <th>Status</th>
    </tr>
  </thead>
  <tbody>
    <tr><td><a href="../backlog_melhorias/#us01">US01</a></td><td>Gestão de Eventos</td><td>Evento com múltiplos dias, inclusive não consecutivos</td><td>RF05, RF11</td><td></td><td>A Fazer</td></tr>
    <tr><td><a href="../backlog_melhorias/#us02">US02</a></td><td>Gestão de Eventos</td><td>Informações completas do evento (valor, vagas, link de pagamento, contato, regulamento)</td><td>RF11</td><td></td><td>A Fazer</td></tr>
    <tr><td><a href="../backlog_melhorias/#us03">US03</a></td><td>Governança e Operações</td><td>Administrador com acesso restrito ao setor pelo qual responde</td><td>RF01</td><td></td><td>A Fazer</td></tr>
    <tr><td><a href="../backlog_melhorias/#us04">US04</a></td><td>Experiência do Usuário e Engajamento</td><td>Página não traduzida automaticamente pelo navegador (corrige foto duplicada)</td><td>RF06, RF04</td><td></td><td>A Fazer</td></tr>
    <tr><td><a href="../backlog_melhorias/#us05">US05</a></td><td>Governança e Operações</td><td>CRUD de líderes e galeria de diretores anteriores pelo painel</td><td>RF02, RF03</td><td></td><td>A Fazer</td></tr>
    <tr><td><a href="../backlog_melhorias/#us06">US06</a></td><td>Gestão de Eventos</td><td>CRUD de bandas e palestrantes, reaproveitáveis entre eventos</td><td>RF47 a RF50</td><td></td><td>A Fazer</td></tr>
  </tbody>
</table>

</center>

### Garantia de Qualidade na Sprint 1

O trabalho de QA ocorre **em paralelo** às histórias acima, não depois delas. As duas primeiras entregas são pré-requisito para que as demais sejam verificadas: enquanto o pipeline não bloquear falha, código com teste quebrado entra na branch principal sem resistência.

<center>

<table border="1" cellspacing="0" cellpadding="4">
  <thead>
    <tr>
      <th>ID</th>
      <th>Entrega</th>
      <th>Cobre</th>
      <th>Responsáveis</th>
      <th>Status</th>
    </tr>
  </thead>
  <tbody>
    <tr><td>QA01</td><td>Tornar os jobs do pipeline bloqueantes no back-end (remoção do <code>continue-on-error</code>)</td><td>Todas</td><td></td><td>A Fazer</td></tr>
    <tr><td>QA02</td><td>Criar workflow de integração contínua no front-end (lint, build e Playwright)</td><td>Todas</td><td></td><td>A Fazer</td></tr>
    <tr><td>QA03</td><td>Testes de regressão do idioma da página e da duplicação de líder sob tradução</td><td>US04</td><td></td><td>A Fazer</td></tr>
    <tr><td>QA04</td><td>Testes de evento com múltiplos dias e dias não consecutivos</td><td>US01, US02</td><td></td><td>A Fazer</td></tr>
    <tr><td>QA05</td><td>Matriz de autorização por papel e testes de negação de acesso</td><td>US03</td><td></td><td>A Fazer</td></tr>
    <tr><td>QA06</td><td>Templates de <em>pull request</em> e de <em>issue</em> com critérios de qualidade</td><td>Processo</td><td></td><td>A Fazer</td></tr>
  </tbody>
</table>

</center>

> **Ordem recomendada.** QA01 e QA02 nos primeiros dias da sprint, por serem trabalho de horas e condição para tudo o mais. QA03 a QA05 acompanham a implementação das histórias correspondentes — o teste é escrito junto da correção, não depois dela.

---

## Como preencher esta página

- **Responsáveis:** cada integrante insere seu nome na linha da história ou entrega que assumir. Mais de um nome por linha é esperado.
- **Requisitos:** os códigos remetem ao catálogo de [Requisitos Elicitados](backlog_melhorias.md#requisitos), onde cada RF e RNF também possui campo de responsável.
- **Status:** `A Fazer`, `Em Andamento`, `Em Revisão` ou `Concluído`.
- Ao encerrar a sprint, duplique a seção para a próxima e registre a alteração no histórico de versão.

As histórias de Prioridade Média e Baixa, assim como os itens fora do escopo, permanecem no [Backlog de Melhorias](backlog_melhorias.md) para planejamento das próximas sprints.

---

## Histórico de Versão

| Versão | Data | Descrição | Autor(es) |
|---|---|---|---|
| `1.0` | 31/08/2026 | Criação da página de Sprints e cadastro da Sprint 1 | [Júlia Massuda](https://github.com/JuliaReis18) |
| `1.1` | 31/08/2026 | Inclusão das colunas de requisitos e responsáveis, das entregas de QA da Sprint 1 e das instruções de preenchimento | [João Pedro](https://github.com/Jadequilin) |
