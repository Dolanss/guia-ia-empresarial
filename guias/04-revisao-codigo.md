# 04 — Como Revisar Código Gerado por IA

Código gerado por IA não vai para produção sem revisão. Ponto. Este guia ensina o que verificar e como fazer essa revisão de forma eficiente.

## Por que revisar é obrigatório

A IA:
- Não sabe o contexto de segurança do seu sistema
- Não sabe quais dados são sensíveis
- Gera código que parece correto mas pode ter falhas sutis
- Não testa o que gera
- Pode copiar padrões desatualizados ou inseguros

**Você é responsável pelo código que sobe.** "A IA gerou" não é justificativa quando algo quebra em produção.

## O checklist de revisão

Use esse checklist antes de qualquer commit de código gerado por IA:

### Segurança (não pule isso)

- [ ] Nenhuma credencial, chave de API ou senha hardcoded no código
- [ ] Dados do usuário são validados antes de usar (forms, inputs, query params)
- [ ] Chamadas de API têm autenticação quando necessário
- [ ] No Supabase: RLS (Row Level Security) está ativado nas tabelas com dados de usuário
- [ ] Nenhuma query SQL que possa retornar dados de outros usuários
- [ ] Erros não expõem informações internas do sistema para o usuário final

### Funcionalidade

- [ ] O código faz o que foi pedido? (teste manualmente)
- [ ] Os casos de erro estão tratados? (o que acontece se a API cair? Se o usuário não tiver permissão?)
- [ ] Estados de carregamento existem onde necessário?
- [ ] O comportamento em mobile está aceitável?

### Qualidade

- [ ] O código está legível? Você consegue entender o que cada parte faz?
- [ ] Não tem código morto (funções criadas mas não usadas)?
- [ ] Os nomes de variáveis e funções fazem sentido?
- [ ] Não tem `console.log` de debug esquecido?

### Dependências

- [ ] Alguma nova biblioteca foi adicionada? Se sim, você sabe para que serve?
- [ ] As versões das dependências estão pinadas (sem `^` para pacotes críticos em produção)?

## Como fazer a revisão na prática

### Passo 1: Leia o diff completo

Não olhe só para o resultado final. Olhe o que mudou.

```bash
git diff
```

Leia cada arquivo modificado. Para cada mudança, pergunte: "Faz sentido? É isso que eu pedi?"

### Passo 2: Teste os casos de erro, não só o caminho feliz

A maioria dos bugs está nos casos de erro. Teste:
- O que acontece com input inválido?
- O que acontece se a rede cair no meio da operação?
- O que acontece se o usuário não tiver permissão?
- O que acontece com campos em branco?

### Passo 3: Peça ao Lovable/Claude para revisar o próprio código

Isso funciona surpreendentemente bem:

```
Revise esse código que você gerou com foco em:
1. Vulnerabilidades de segurança
2. Casos de erro não tratados
3. Dados de usuário que poderiam ser expostos

[cole o código]
```

Use a resposta como ponto de partida — não como aprovação final.

### Passo 4: Verifique o Supabase (se aplicável)

Se o projeto usa Supabase, verifique no dashboard:
- **Authentication:** As políticas de acesso estão corretas?
- **Table Editor:** RLS está ativado em todas as tabelas com dados de usuário?
- **API:** Alguma tabela está exposta sem proteção?

## Revisão rápida vs. revisão completa

**Revisão rápida** (para mudanças pequenas, sem risco de segurança):
- Leia o diff
- Teste manualmente a mudança
- Verifique os itens de segurança relevantes

**Revisão completa** (para features novas, mudanças em autenticação, qualquer coisa que vai para produção pela primeira vez):
- Todo o checklist acima
- Teste em ambiente separado antes de produção
- Um segundo par de olhos se possível

## Red flags — pare tudo se ver isso

| O que você vê | O que fazer |
|---------------|-------------|
| `password`, `secret`, `key` com valor hardcoded | Remova imediatamente, use variável de ambiente |
| `SELECT *` sem `WHERE user_id = $1` em tabela de usuário | Verifique se pode retornar dados de outro usuário |
| Endpoint sem verificação de autenticação | Adicione verificação antes de subir |
| `eval()`, `dangerouslySetInnerHTML` com input do usuário | Riscos de XSS — entenda antes de usar |
| `rm -rf`, `DROP TABLE`, operações destrutivas | Confirme duplamente antes de executar |
| Dependência que você não reconhece | Pesquise antes de instalar |

## A regra dos dois olhos

Código que vai para produção deve ser lido por pelo menos uma pessoa que não o gerou. Isso vale para código humano, vale para código de IA.

Se você é a única pessoa que vai ler, pelo menos dê um intervalo de alguns minutos entre gerar e revisar — distância temporal ajuda a pegar erros óbvios.

Próximo: [Segurança e o que nunca fazer →](05-seguranca.md)
