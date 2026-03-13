# Template: Revisão de Código

Use quando quiser pedir ao Claude para revisar código gerado por IA (ou por qualquer pessoa) antes de subir.

---

```
## Revisão de Código

### Contexto
[O que esse código faz? Qual problema resolve?]

### Foco da revisão
Revise com foco em:
- [ ] Segurança (credenciais, validação de input, autorização)
- [ ] Casos de erro não tratados
- [ ] Lógica incorreta ou edge cases
- [ ] Qualidade e legibilidade
- [ ] Performance (apenas se obviamente ruim)

### Código para revisar

```[linguagem]
[cole o código aqui]
```

### Restrições de contexto
- Stack: [sua stack]
- [qualquer outra informação relevante para a revisão]

### O que NÃO revisar
[se houver partes que você sabe que estão OK ou são intencionais, mencione aqui]

### Formato do resultado
Para cada problema encontrado:
- Onde está (arquivo:linha se souber, ou descrição)
- O que está errado
- Como corrigir
- Nível de gravidade: crítico / importante / sugestão
```

---

## Prompt rápido de revisão de segurança

Para uma revisão rápida focada em segurança (copie e cole diretamente):

```
Revise esse código com foco exclusivo em segurança. Para cada problema encontrado,
informe: onde está, qual é o risco, e como corrigir. Classifique como crítico
(deve corrigir antes de subir), importante (deve corrigir em breve) ou baixo risco.

Preste atenção especial em:
- Credenciais ou segredos hardcoded
- Input do usuário usado sem validação
- Queries que podem retornar dados de outros usuários
- Endpoints sem verificação de autenticação
- Dados sensíveis em logs ou mensagens de erro

[cole o código]
```

---

## Exemplo de revisão Supabase + RLS

```
Revise essa configuração de banco de dados Supabase. O projeto é um app
onde usuários criam e gerenciam seus próprios projetos. Cada usuário deve
ver apenas seus próprios dados.

Verifique:
1. RLS está ativado nas tabelas corretas?
2. As políticas permitem que usuários acessem só seus próprios dados?
3. Existe alguma brecha onde um usuário poderia ler dados de outro?

SQL das tabelas e políticas:
[cole o SQL das suas tabelas e políticas de RLS]
```
