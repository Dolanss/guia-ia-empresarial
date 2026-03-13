# Template: Contexto de Projeto

Use esse template no início de qualquer conversa com IA sobre um projeto existente.
Cole o conteúdo preenchido como **primeiro mensagem** antes de qualquer pedido.

---

## Como usar

1. Copie o bloco abaixo
2. Preencha todos os campos (remova os comentários em parênteses)
3. Cole como primeiro mensagem no Lovable, Claude, ou ChatGPT
4. Faça seu pedido específico logo depois

---

```
## Contexto do Projeto

**Nome:** [nome do projeto]
**Objetivo:** [o que o produto faz, em uma frase]
**Status:** [em desenvolvimento / em produção / prototipando]

## Stack Técnica

- **Frontend:** [ex: React 18 + TypeScript + Vite + Tailwind CSS]
- **Backend/BaaS:** [ex: Supabase / Node.js + Express / nenhum]
- **Banco de dados:** [ex: PostgreSQL via Supabase / SQLite / nenhum]
- **Deploy:** [ex: Vercel / Netlify / AWS / ainda não decidido]
- **Autenticação:** [ex: Supabase Auth / NextAuth / sem auth]

## Estrutura de Pastas Principal

[cole a estrutura relevante, ex:]
src/
├── components/
│   └── ui/          ← componentes reutilizáveis
├── pages/           ← uma pasta por tela
├── lib/             ← utilitários e configuração
└── types/           ← interfaces TypeScript

## Convenções que Seguimos

- [ex: Componentes em PascalCase, funções em camelCase]
- [ex: CSS via classes Tailwind, sem CSS inline]
- [ex: Sem bibliotecas novas sem aprovação do time]
- [ex: Comentários em português, código em inglês]

## O que NÃO Fazer

- [ex: Não criar novos arquivos fora das pastas existentes]
- [ex: Não modificar [arquivo específico] — tem lógica crítica]
- [ex: Não instalar dependências novas]
- [ex: Não alterar o banco de dados diretamente]

## Contexto Adicional

[qualquer outra informação relevante: decisões de design, restrições de negócio,
usuário-alvo, integrações externas que existem, etc.]
```

---

## Exemplo preenchido

```
## Contexto do Projeto

**Nome:** Portal RH Interno
**Objetivo:** Centralizar solicitações de RH dos funcionários (férias, reembolso, documentos)
**Status:** Em desenvolvimento

## Stack Técnica

- **Frontend:** React 18 + TypeScript + Vite + Tailwind CSS
- **Backend/BaaS:** Supabase
- **Banco de dados:** PostgreSQL via Supabase
- **Deploy:** Vercel
- **Autenticação:** Supabase Auth com login por e-mail corporativo

## Estrutura de Pastas Principal

src/
├── components/
│   ├── ui/          ← Button, Input, Modal, Card (não modificar)
│   └── forms/       ← formulários de solicitação
├── pages/           ← Dashboard, Solicitacoes, Perfil
├── lib/
│   ├── supabase.ts  ← cliente Supabase (não modificar)
│   └── utils.ts     ← funções utilitárias
└── types/
    └── index.ts     ← interfaces de dados

## Convenções que Seguimos

- Componentes em PascalCase, hooks com prefixo "use"
- Sempre TypeScript, sem "any" explícito
- Tailwind para tudo, sem styled-components ou CSS modules
- Dados de usuário sempre filtrados por user_id na query

## O que NÃO Fazer

- Não modificar lib/supabase.ts
- Não criar tabelas novas no Supabase sem discutir primeiro
- Não instalar bibliotecas novas de UI (usamos só o Tailwind)

## Contexto Adicional

- Usuários são funcionários com login corporativo (@empresa.com)
- Cada funcionário vê só suas próprias solicitações
- Gestores veem as solicitações do time deles
- Notificações por e-mail ainda não implementadas (próxima fase)
```
