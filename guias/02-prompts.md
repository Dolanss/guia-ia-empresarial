# 02 — Como Escrever Prompts que Funcionam

A qualidade do seu prompt determina 80% do resultado. Isso não é exagero.

## A estrutura de um bom prompt

Um prompt eficaz tem quatro partes:

```
[CONTEXTO] O que já existe / qual é a situação atual
[OBJETIVO] O que você quer que aconteça
[RESTRIÇÕES] O que não pode fazer / limites
[FORMATO] Como quer o resultado
```

Você não precisa escrever as etiquetas. Mas precisa ter as quatro partes mentalmente antes de escrever.

### Exemplo ruim vs. bom

**Ruim:**
```
Cria um formulário de login
```

**Bom:**
```
Contexto: Tenho um projeto Next.js 14 com App Router e Tailwind CSS.
A autenticação usa NextAuth com provider Google. O formulário precisa
seguir o design system já existente (componentes em /components/ui/).

Objetivo: Criar uma página de login em /app/login/page.tsx com campos
de email e senha, além do botão "Entrar com Google".

Restrições: Não criar novos componentes — usar os que já existem em
/components/ui/. Não instalar novas dependências.

Formato: Apenas o código do arquivo, sem explicação.
```

O segundo prompt elimina ambiguidades. A IA não precisa adivinhar nada.

## As perguntas que você deve responder antes de escrever

Antes de pedir qualquer coisa, responda:

1. **Qual é meu stack?** (linguagem, framework, versão)
2. **O que já existe no projeto?** (estrutura de pastas, padrões, componentes)
3. **Quais são as restrições?** (sem nova lib, seguir padrão X, não mexer em Y)
4. **O que é "pronto"?** (como eu vou saber que o resultado está correto?)
5. **Qual o escopo?** (um arquivo? Uma função? Uma feature inteira?)

## Princípios

### 1. Um pedido de cada vez

Pedir múltiplas coisas gera resultado mediano em tudo.

**Ruim:** "Cria o formulário, conecta com a API, adiciona validação e testa tudo"

**Bom:** Pedir cada parte separadamente, validar uma antes de partir para a próxima.

---

### 2. Seja específico sobre o "não"

A IA vai sempre adicionar coisas que você não pediu se você não proibir.

```
Não adicione comentários no código.
Não crie arquivos extras.
Não use bibliotecas além das já listadas no package.json.
Não altere arquivos que eu não mencionar.
```

---

### 3. Mostre, não só descreva

Em vez de descrever o que você quer, mostre exemplos.

```
Siga esse padrão de componente:

export function Button({ children, onClick, variant = 'primary' }) {
  return (
    <button
      onClick={onClick}
      className={variants[variant]}
    >
      {children}
    </button>
  )
}

Crie o componente Input seguindo o mesmo padrão.
```

---

### 4. Dê o erro completo, não a descrição do erro

**Ruim:** "Está dando erro de importação"

**Bom:**
```
Recebo esse erro ao rodar npm run dev:

Module not found: Can't resolve '@/components/ui/button'
  at /app/dashboard/page.tsx:3

O arquivo existe em src/components/ui/Button.tsx.
O tsconfig.json tem o alias @ apontando para src/.
O que está errado?
```

---

### 5. Corrija com precisão cirúrgica

Quando algo está errado, não diga "corrija isso" — diga **o quê** está errado e **por quê**.

**Ruim:** "Isso não está funcionando, conserta"

**Bom:** "A função `calcularTotal` está retornando o valor sem aplicar o desconto. Na linha 23, o desconto deveria ser subtraído antes de multiplicar pela quantidade. Corrija apenas essa função."

---

### 6. Recomece quando a conversa desandar

Conversas longas degradam a qualidade. Se você está no décimo "isso ainda não funcionou", **abra uma nova conversa** e comece com o contexto limpo e o que já sabe que não funciona.

Continuar na mesma conversa ruim raramente resolve.

## Padrões de prompt por situação

### Para criar algo do zero
```
Stack: [sua stack]
Contexto: [o que já existe]
Quero criar: [o que você quer]
Deve seguir: [padrão/convenção]
Não pode: [restrições]
Formato de output: [só código / com explicação / etc]
```

### Para corrigir um problema
```
O que deveria acontecer: [comportamento esperado]
O que está acontecendo: [comportamento atual]
Erro: [mensagem completa de erro, se houver]
Código relevante: [trecho onde está o problema]
O que já tentei: [o que não funcionou]
```

### Para refatorar
```
Código atual: [código]
Problema com o código atual: [o que está ruim]
Manter: [o que não pode mudar]
Objetivo: [o que deve melhorar]
```

### Para entender código
```
Explique o que esse código faz, linha por linha:
[código]

Foque em: [aspecto específico que você quer entender]
```

## O que não funciona

| Prompt | Por que falha |
|--------|---------------|
| "Melhore isso" | "Melhor" é subjetivo — a IA vai fazer escolhas arbitrárias |
| "Está quebrado, arruma" | Sem contexto do erro, a IA vai chutar |
| "Cria o sistema inteiro" | Escopo enorme = resultado genérico e inconsistente |
| "Como sempre fazemos aqui" | A IA não sabe como vocês fazem |
| "Agora faça funcionar" | Sem especificar o quê "funcionar" significa |

Próximo: [Guia específico do Lovable →](03-lovable.md)
