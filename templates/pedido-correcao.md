# Template: Pedido de Correção

Use quando algo está quebrado ou se comportando errado. Preencha antes de escrever o prompt.

O segredo é ser preciso sobre **o que está errado** e **qual deveria ser o comportamento correto**.

---

```
## Problema: [descrição curta em uma linha]

### Comportamento atual
[o que está acontecendo de errado, com máximo de detalhe]

### Comportamento esperado
[o que deveria acontecer]

### Como reproduzir
1. [passo 1]
2. [passo 2]
3. [onde o erro aparece]

### Mensagem de erro (se houver)
[cole a mensagem de erro completa, incluindo stack trace]

### Código relevante
[cole o trecho de código onde o problema está, se souber]

### O que já tentei
[o que você já tentou fazer que não funcionou — isso evita que a IA
sugira as mesmas coisas]

### Contexto
- **Ambiente:** [desenvolvimento local / staging / produção]
- **Quando começou:** [sempre foi assim / começou depois de X]
- **Frequência:** [sempre / às vezes / em condição específica]

### Escopo da correção
Corrija apenas [o problema específico]. Não altere [o que não deve mudar].
```

---

## Exemplo preenchido

```
## Problema: Botão "Salvar" não funciona no formulário de perfil

### Comportamento atual
Ao clicar em "Salvar" no formulário de perfil, nada acontece.
O botão fica cinza por um segundo e volta ao normal, mas os dados
não são salvos e nenhuma mensagem de confirmação ou erro aparece.

### Comportamento esperado
Ao clicar em "Salvar", os dados do formulário são enviados para o
Supabase, o usuário vê uma mensagem "Perfil atualizado com sucesso"
e os dados novos aparecem na tela.

### Como reproduzir
1. Fazer login
2. Ir em Configurações → Perfil
3. Alterar o campo "Nome completo"
4. Clicar em "Salvar"
5. Nada acontece

### Mensagem de erro (se houver)
No console do browser:
  TypeError: Cannot read properties of undefined (reading 'id')
    at updateProfile (ProfileForm.tsx:47)
    at handleSubmit (ProfileForm.tsx:31)

### Código relevante
// ProfileForm.tsx linha 40-52
async function updateProfile(data: ProfileData) {
  const { error } = await supabase
    .from('profiles')
    .update(data)
    .eq('id', user.id)  // ← user pode estar undefined aqui?

  if (error) {
    console.error(error)
  }
}

### O que já tentei
- Adicionei console.log antes da chamada: user aparece como undefined
- Verifiquei que o login funciona (consigo ver o dashboard)
- O problema aparece só no formulário de perfil, não em outros formulários

### Contexto
- **Ambiente:** Desenvolvimento local
- **Quando começou:** Após refatorar o hook useAuth ontem
- **Frequência:** Sempre

### Escopo da correção
Corrija apenas o problema com user.id undefined. Não altere o resto
do formulário, os campos estão funcionando corretamente.
```
