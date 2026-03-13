# 03 — Lovable: Guia Prático

O Lovable é uma ferramenta que gera aplicações web completas a partir de prompts. É poderoso e também muito fácil de usar mal. Este guia cobre o fluxo correto, as armadilhas mais comuns e como tirar resultado real dele.

## O que o Lovable faz

O Lovable gera código React/TypeScript com Tailwind CSS, conecta com Supabase para backend/banco, e pode integrar com várias APIs. Ele não só gera um arquivo — ele monta projetos inteiros com estrutura de pastas, configuração e dependências.

**Isso é ótimo para:** protótipos, MVPs, dashboards internos, formulários complexos, landing pages com lógica.

**Isso não é para:** sistemas legados que precisam de integração cirúrgica, código que vai ser mantido por um time sem entender o que foi gerado, substituição de decisão de arquitetura.

## O fluxo correto

### 1. Defina antes de gerar

Antes de abrir o Lovable, responda no papel:

- O que esse produto faz? (uma frase)
- Quem usa? (persona)
- Quais são as telas principais?
- Quais dados precisam persistir?
- Quais integrações externas?

Sem isso definido, você vai gerar, jogar fora e gerar de novo indefinidamente.

### 2. Comece com contexto completo

O primeiro prompt é o mais importante. Invista tempo nele.

**Template de primeiro prompt:**

```
Quero criar [nome do produto]: [descrição em uma frase].

Usuário principal: [quem usa]

Funcionalidades principais:
1. [funcionalidade 1]
2. [funcionalidade 2]
3. [funcionalidade 3]

Telas necessárias:
- [tela 1]: [o que faz]
- [tela 2]: [o que faz]

Stack preferida: React + TypeScript + Tailwind + Supabase

Design: [minimalista/moderno/corporativo/etc], tema [claro/escuro],
cor primária [cor]

Não incluir: [o que você explicitamente não quer]
```

### 3. Itere em pequenos passos

Após o projeto inicial gerado, **não peça tudo de uma vez**. Trabalhe feature por feature:

```
✅ "Adicione validação no formulário de cadastro: email obrigatório,
   senha mínimo 8 caracteres, confirmação de senha deve bater"

❌ "Adiciona validação em tudo, melhora o design, conecta com API
   de pagamento e cria o painel de admin"
```

### 4. Teste cada passo antes do próximo

Antes de pedir qualquer mudança nova:
- Abra o preview
- Teste o que foi gerado
- Anote o que está errado com precisão
- Só então faça o próximo pedido

Pular essa etapa é a causa número um de projetos Lovable que viram espaguete.

## Como dar feedback eficaz

### Feedback sobre visual

**Ruim:** "Está feio, muda o design"

**Bom:**
```
O header está com muita altura — reduza o padding vertical de 24px para 12px.
O botão primário deveria ser azul (#2563EB), não verde.
A tabela está cortando na borda direita em mobile — adicione overflow-x: auto.
```

### Feedback sobre comportamento

**Ruim:** "O login não funciona"

**Bom:**
```
Ao clicar em "Entrar", o console mostra:
  AuthError: Invalid login credentials

O email e senha que estou usando são válidos no Supabase (testei
direto no dashboard dele). O problema parece ser na chamada do
signInWithPassword. Verifique se as credenciais do Supabase estão
corretas no .env e como a função está sendo chamada.
```

### Feedback sobre lógica

**Ruim:** "O cálculo está errado"

**Bom:**
```
A função de cálculo de desconto está errada. Para um pedido de R$100
com 10% de desconto, está retornando R$90 mas deveria retornar R$10
(o valor do desconto, não o valor final). Corrija a função calcularDesconto
para retornar o valor do desconto, não o valor após desconto.
```

## Armadilhas comuns

### Armadilha 1: "Só mais uma coisa"

Você pede uma mudança, funciona. Pede outra, ainda funciona. Pede mais uma... e o Lovable começa a quebrar o que estava certo. Isso acontece porque o contexto da conversa fica grande e inconsistente.

**Solução:** A cada 5-7 mudanças grandes, crie um novo projeto com o código atual como base. Ou use o sistema de branches do Lovable.

---

### Armadilha 2: Não entender o que foi gerado

Você pede, funciona, você sobe. Três semanas depois algo quebra e ninguém sabe por quê porque ninguém leu o código.

**Solução:** Antes de qualquer deploy, leia o código gerado. Pelo menos as partes críticas (autenticação, manipulação de dados, chamadas de API). Se não consegue ler, peça ao Lovable: "Explique o que esse código faz e quais são os pontos de atenção."

---

### Armadilha 3: Colocar segredos no prompt

"Conecta com minha API usando a chave sk-prod-xxxxx"

**Nunca faça isso.** Seu prompt pode aparecer em logs, histórico, ou ser usado para treinar modelos. Use variáveis de ambiente.

---

### Armadilha 4: Escopo sem fim

"Agora adiciona isso... e isso... e aquilo também..."

O Lovable não tem senso de escopo de produto. Ele vai adicionar tudo que você pedir. Isso gera projetos gigantes, inconsistentes e impossíveis de manter.

**Solução:** Defina o MVP antes de começar. Só peça o que está no MVP. Tudo que não está no MVP vai para uma lista separada.

---

### Armadilha 5: Usar em produção sem revisão

O Lovable gera código funcional na maior parte do tempo. Mas ele pode gerar:
- Queries SQL que expõem dados de outros usuários (falta de RLS no Supabase)
- Formulários sem validação adequada
- Endpoints sem autenticação
- Dependências com vulnerabilidades conhecidas

**Solução:** Veja [`guias/04-revisao-codigo.md`](04-revisao-codigo.md) antes de qualquer deploy.

## Recursos do Lovable que muita gente ignora

**Supabase integrado:** O Lovable conecta direto com Supabase. Use isso — não tente reinventar backend custom.

**Deploy com um clique:** O Lovable faz deploy direto. Mas entenda o que está sendo deployado antes de apertar o botão.

**Visual edits:** Você pode clicar em elementos do preview e editar diretamente, sem escrever prompt. Muito mais preciso para ajustes visuais.

**Histórico de versões:** Se algo quebrou, você pode voltar para uma versão anterior. Use isso.

**Modo de código:** Você pode ver e editar o código diretamente. Use isso para entender o que foi gerado.

## Quando parar de usar o Lovable

O Lovable é ótimo para começar. Em algum momento você vai precisar sair dele:

- Quando o projeto crescer além do que um contexto de conversa consegue gerenciar
- Quando precisar de integrações muito customizadas
- Quando o time precisar trabalhar em paralelo com git, branches e pull requests
- Quando tiver requisitos de segurança/compliance que precisam de auditoria técnica

Nesse ponto, exporte o código e continue em um ambiente de desenvolvimento normal.

Próximo: [Como revisar código gerado por IA →](04-revisao-codigo.md)
