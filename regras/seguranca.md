# Regras de Segurança

Estas regras se aplicam a todo uso de IA na empresa. Não são sugestões.

## Proibições absolutas

### Nunca coloque no prompt:
- Chaves de API (AWS, Google Cloud, Stripe, Supabase, etc.)
- Senhas de banco de dados ou outros serviços
- Tokens de acesso ou refresh tokens
- Chaves privadas ou certificados
- Connection strings com credenciais
- Dados pessoais de clientes (CPF, e-mail, telefone, endereço)
- Dados financeiros de clientes (número de cartão, dados bancários)
- Qualquer dado coberto pela LGPD de usuários reais

### Nunca faça:
- Deploy de código gerado por IA sem revisão
- Commit de código com credenciais hardcoded (mesmo que pareça teste)
- Dar permissões de admin para ferramentas de IA quando não necessário
- Usar dados reais de produção para testar features com IA

## O que fazer com credenciais

Sempre use variáveis de ambiente. No prompt, descreva a variável:

```
Configure a conexão com o Supabase usando as variáveis de ambiente
SUPABASE_URL e SUPABASE_ANON_KEY que já estão definidas no .env.
```

Confirme que o `.env` está no `.gitignore` antes de qualquer commit.

## Verificação obrigatória antes de deploy

Execute esse comando antes de qualquer commit:

```bash
# Procura por possíveis credenciais no código
grep -r "password\s*=\|secret\s*=\|api_key\s*=\|sk-\|pk-" \
  --include="*.ts" --include="*.tsx" --include="*.js" --include="*.env*" .
```

Se encontrar algo suspeito, investigue antes de continuar.

## Supabase: verificação de RLS

Para qualquer projeto com Supabase que vai para produção:

1. Acesse o painel do Supabase → Authentication → Policies
2. Para cada tabela com dados de usuário, confirme que há uma política de leitura que limita ao próprio usuário
3. Teste com dois usuários diferentes: usuário B não pode ver dados do usuário A

## Incidente de segurança

Se você perceber que uma credencial foi exposta (colada num prompt, commitada no git, etc.):

1. **Revogue imediatamente** a credencial no serviço correspondente
2. **Gere uma nova** credencial
3. **Atualize** todos os ambientes que usam a credencial antiga
4. **Documente** o incidente (o que foi exposto, quando, como foi resolvido)
5. **Informe** o responsável pela segurança da empresa

Não espere para ver se "alguém vai usar". Revogue imediatamente.
