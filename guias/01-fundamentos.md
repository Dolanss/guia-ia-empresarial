# 01 — Fundamentos: Como a IA Funciona

Você não precisa entender os bastidores matemáticos para usar bem. Mas entender o modelo mental certo faz toda a diferença.

## O que a IA faz (de verdade)

Ferramentas como Claude, ChatGPT e o motor por trás do Lovable são **modelos de linguagem**. Eles predizem qual texto faz mais sentido vir depois do que você escreveu. Não "pensam" — completam padrões com base em tudo que foram treinados.

**Consequências práticas:**

- O modelo não sabe o que você quer, só o que você escreveu
- Ele vai "completar" com o que parece mais provável, não o que é mais correto
- Contexto é tudo — quanto mais você der, melhor a completação

## O que a IA é boa em fazer

| Boa em | Fraca em |
|--------|---------|
| Gerar código boilerplate | Entender requisitos vagos |
| Traduzir entre linguagens | Saber o que você não disse |
| Explicar conceitos | Verificar se o resultado funciona |
| Reformatar e refatorar | Manter consistência em projetos grandes |
| Sugerir abordagens | Decidir qual abordagem é certa para seu contexto |
| Encontrar erros quando mostrada o código | Saber o que seu sistema faz sem ver o código |

## Por que os resultados são inconsistentes

Você já passou por isso: pede a mesma coisa duas vezes e recebe respostas diferentes. Ou funciona na primeira vez e quebra tudo na segunda.

**Motivos comuns:**

1. **Você mudou o contexto sem perceber** — cada conversa começa do zero (a maioria das ferramentas não lembra da anterior)
2. **O prompt foi ambíguo** — a IA "escolheu" uma interpretação diferente
3. **O contexto ficou grande demais** — em conversas longas, o modelo perde o fio das instruções iniciais
4. **Você pediu muita coisa de uma vez** — a IA prioriza partes do pedido e ignora outras

## O ciclo problemático (e como sair dele)

```
Prompt vago
    ↓
Resultado ruim
    ↓
"Corrija isso" (sem explicar o quê e por quê)
    ↓
Resultado diferente, mas ainda ruim
    ↓
Frustração
```

**O ciclo produtivo:**

```
Contexto claro + pedido específico
    ↓
Resultado parcialmente certo
    ↓
Revisão precisa: "X está errado porque Y. Corrija apenas X."
    ↓
Resultado melhor
    ↓
Teste e validação humana
```

## Contexto é o insumo mais importante

A IA não sabe:
- Qual é a sua stack
- Qual é o padrão de código do seu time
- O que já existe no projeto
- O que você tentou antes e não funcionou
- Qual é a restrição de negócio

**Tudo isso você precisa fornecer.** Quanto mais contexto relevante, mais útil é o resultado.

Veja como estruturar isso em [`guias/02-prompts.md`](02-prompts.md).

## Sobre "alucinação"

A IA inventa coisas com confiança. Isso se chama alucinação. Ela vai:
- Citar funções que não existem
- Descrever comportamentos errados de bibliotecas
- Gerar código que parece certo mas não funciona
- Afirmar fatos incorretos com certeza

**Nunca assuma que o output está correto sem verificar.** Código gerado por IA precisa ser lido, testado e validado — não só colado e commitado.

## A analogia certa

Pense na IA como um **estagiário muito rápido com memória de ouro, mas sem julgamento próprio**:

- Ele executa qualquer instrução sem questionar se faz sentido
- Ele não sabe o que não foi dito
- Ele produz código que parece profissional mas pode ter falhas escondidas
- Você precisa revisar o trabalho dele antes de subir para produção
- Ele fica melhor conforme você dá mais contexto e feedback preciso

Próximo: [Como escrever prompts que funcionam →](02-prompts.md)
