# Exemplos: Armadilhas Comuns

Os erros mais frequentes ao usar IA para desenvolvimento. Cada um vem com o que acontece e como evitar.

---

## 1. O prompt vago que gera código genérico

**O que fazem:**
```
cria um dashboard
```

**O que acontece:** A IA gera um dashboard genérico com dados fictícios, componentes que não existem no seu projeto, e uma estrutura completamente diferente do que você tem.

**O que fazer em vez disso:**
```
Cria uma página de dashboard em src/pages/Dashboard.tsx.

Dados para exibir (vêm da tabela `pedidos` no Supabase):
- Total de pedidos do mês (COUNT)
- Valor total do mês (SUM de valor)
- Pedidos pendentes (WHERE status = 'pendente')

Componentes existentes para usar:
- Card em src/components/ui/Card.tsx (props: title, value, description)
- já tem um hook usePedidos em src/hooks/usePedidos.ts

Layout: 3 cards no topo, depois a tabela de pedidos recentes.
```

---

## 2. Acumular erros sem limpar o contexto

**O que fazem:** Na mesma conversa:
- "Não funcionou, tenta de novo"
- "Ainda não funcionou"
- "O que está errado?"
- "Você não entendeu, era pra fazer X"
- ...20 mensagens depois...
- "Por que nada funciona?"

**O que acontece:** O modelo fica confuso com as instruções contraditórias da conversa longa, começa a gerar soluções que contradizem soluções anteriores, e a qualidade cai progressivamente.

**O que fazer em vez disso:** Depois de 3-4 tentativas sem sucesso, **abra uma nova conversa**. Comece com o contexto limpo, inclua o que você sabe que NÃO funciona, e peça de forma mais específica.

---

## 3. Pedir para "melhorar" sem definir o que "melhor" significa

**O que fazem:**
```
Refatora esse código para ficar melhor
```

**O que acontece:** A IA refatora seguindo critérios próprios — talvez adicione abstrações desnecessárias, mude o estilo de programação, ou "melhore" coisas que estavam deliberadamente feitas de certa forma.

**O que fazer em vez disso:**
```
Refatora esse código para:
1. Extrair a lógica de validação para uma função separada chamada validateForm
2. Remover o estado redundante (selectedItems e selectedCount fazem a mesma coisa)
3. Não alterar nada mais além disso
```

---

## 4. Confiar no "funciona" sem testar

**O que fazem:** Lovable gera, preview parece OK, fazem deploy.

**O que acontece:** Em produção:
- O formulário não salva dados reais (estava usando dados mock)
- Qualquer usuário pode ver os dados de todos os outros (sem RLS)
- O botão funciona no desktop mas não no mobile
- Funciona com email, não funciona com Google OAuth

**O que fazer em vez disso:** Antes de qualquer deploy, envie para o QA validar:
1. Testar com dados reais (não só o happy path)
2. Testar casos de erro (o que acontece se a rede cair? Se o campo estiver vazio?)
3. Verificar no Supabase se os dados chegaram corretamente
4. Testar em mobile

---

## 5. Pedir a feature inteira de uma vez

**O que fazem:**
```
Cria o módulo completo de autenticação com login, cadastro, recuperação
de senha, verificação de email, perfil do usuário e configurações de conta
```

**O que acontece:** A IA gera algo enorme, provavelmente inconsistente internamente, com partes que não conectam com o resto do projeto.

**O que fazer em vez disso:** Quebre em etapas. Implemente e teste cada uma antes de partir para a próxima:

1. Primeiro: página de login com e-mail/senha
2. Teste o login
3. Depois: página de cadastro
4. Teste o cadastro
5. Depois: recuperação de senha
6. ...

---

## 6. Ignorar os erros do console

**O que fazem:** O app funciona visualmente, mas o console está cheio de:
- `Warning: Each child in a list should have a unique "key" prop`
- `Warning: Can't perform a React state update on an unmounted component`
- `TypeError: Cannot read properties of undefined`

Eles ignoram porque "não parece afetar nada".

**O que acontece:** Esses warnings indicam problemas reais que vão virar bugs em produção, problemas de performance, ou vazamentos de memória.

**O que fazer em vez disso:** Warnings são bugs em espera. Cole no prompt e peça para corrigir.

---

## 7. Colar código em produção sem ler

**O que fazem:** Recebem o código, parece correto, colam no arquivo, commit, push.

**O que acontece:**
- O código tem `console.log('debug:', userData)` esquecido (vaza dados sensíveis nos logs)
- Uma variável chamada `password` aparece no código com um valor de exemplo
- A IA adicionou uma dependência nova sem avisar

**O que fazer em vez disso:** Envie o código para o QA revisar antes de commitar. Sempre. O QA vai escanear por coisas suspeitas e validar antes da promoção.

---

## 8. Usar IA para tomar decisões de arquitetura

**O que fazem:**
```
Qual é a melhor arquitetura para meu sistema?
```

**O que acontece:** A IA sugere algo genérico, sem conhecer seu time, seus requisitos reais, suas restrições de prazo, e o nível técnico de quem vai manter.

**O que fazer em vez disso:** Use IA para **explorar opções** com você, não para **decidir por você**:
```
Estou decidindo entre usar Supabase ou um backend Node.js próprio para
um sistema de e-commerce pequeno (menos de 1000 usuários/mês). Meu time
tem 2 devs frontend sem experiência em backend. Compare as duas opções
nos critérios: custo de desenvolvimento, custo de manutenção, escalabilidade,
e curva de aprendizado.
```

A decisão final é sua, com base no que você conhece do contexto.

---

## 9. Não salvar o contexto entre sessões

**O que fazem:** Trabalham no projeto hoje, no Lovable ou Claude. Amanhã, abrem uma nova conversa e precisam reexplicar tudo do zero.

**O que acontece:** Perda de tempo explicando contexto repetidamente. A IA não vai lembrar de decisões tomadas antes.

**O que fazer em vez disso:** Use o [`templates/contexto-projeto.md`](../templates/contexto-projeto.md). Mantenha um arquivo com o contexto do projeto atualizado e cole no início de cada nova conversa.

---

## 10. Pedir para a IA corrigir o código dela sem mostrar o erro

**O que fazem:**
```
Isso não funcionou
```

**O que acontece:** A IA tenta adivinhar o que pode ter dado errado e gera uma variação do mesmo código. 50% de chance de ser igual, 50% de ser diferente mas ainda errado.

**O que fazer em vez disso:** Sempre inclua:
1. O erro completo (mensagem + stack trace)
2. O comportamento que você observou
3. O que você esperava que acontecesse
