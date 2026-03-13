# 05 — Segurança: O Que Nunca Fazer com IA

Este guia cobre riscos reais que acontecem quando pessoas usam IA sem pensar nas implicações de segurança. Leia antes de colocar qualquer coisa em produção.

## Regras absolutas

Essas são proibições, não sugestões:

### 1. Nunca coloque credenciais no prompt

Isso inclui:
- Chaves de API (AWS, Google, Stripe, Supabase, etc.)
- Senhas de banco de dados
- Tokens de acesso (OAuth, JWT, etc.)
- Chaves privadas
- Connection strings com usuário e senha

**Por quê:** Prompts podem aparecer em logs, histórico de conversa, e potencialmente em dados de treinamento de modelos. Uma chave vazada pode comprometer todo o sistema.

**O que fazer em vez disso:**
```
Use variáveis de ambiente. No prompt, descreva a variável:
"Configure a conexão com o banco usando a variável de ambiente DATABASE_URL"
```

---

### 2. Nunca cole dados pessoais de clientes no prompt

Isso inclui:
- CPF, RG, documentos de identidade
- Dados financeiros (número de cartão, conta bancária)
- Informações médicas
- E-mails, telefones, endereços de clientes reais
- Qualquer dado coberto pela LGPD

**Por quê:** Você não sabe onde esses dados vão parar. Além do risco de segurança, é uma potencial violação de LGPD.

**O que fazer em vez disso:** Use dados fictícios para testar e desenvolver. Exemplo: `CPF: 000.000.000-00`, `email: teste@exemplo.com`.

---

### 3. Nunca use código gerado sem ler

Especialmente para código que:
- Lida com autenticação e autorização
- Acessa banco de dados
- Processa pagamentos
- Envia e-mails ou notificações
- Faz upload ou download de arquivos
- Deleta dados

---

### 4. Nunca suba para produção sem testar

"Funcionou no preview" não é teste. Teste em ambiente separado (staging/homologação) antes.

---

### 5. Nunca dê permissões desnecessárias

Se o Lovable pede para conectar com sua conta do Google, AWS, ou qualquer serviço externo, dê **só as permissões mínimas necessárias**. Não dê acesso de admin quando acesso de leitura é suficiente.

## Riscos específicos do Lovable + Supabase

A combinação Lovable + Supabase é muito comum. Esses são os problemas mais frequentes:

### RLS desabilitado

Row Level Security (RLS) no Supabase controla quem pode ver quais dados. Quando o Lovable cria tabelas, o RLS às vezes fica desabilitado por padrão.

**Consequência:** Qualquer usuário autenticado pode ler os dados de todos os outros usuários.

**Como verificar:**
1. Abra seu projeto no Supabase
2. Vá em Authentication → Policies
3. Para cada tabela com dados de usuário, confirme que há políticas de RLS

**Como pedir ao Lovable para corrigir:**
```
Verifique se todas as tabelas com dados de usuário têm RLS ativado
no Supabase. Para a tabela [nome], adicione uma política que permita
usuários ler e modificar apenas seus próprios dados (WHERE user_id = auth.uid()).
```

### API key exposta no frontend

Supabase tem duas chaves: `anon key` (pode ficar no frontend) e `service_role key` (nunca no frontend). Confirme que o código usa a `anon key` no cliente.

### Queries sem filtro de usuário

```sql
-- Errado: retorna dados de todos os usuários
SELECT * FROM pedidos WHERE status = 'ativo'

-- Correto: retorna só os dados do usuário atual
SELECT * FROM pedidos WHERE status = 'ativo' AND user_id = auth.uid()
```

## O que fazer quando suspeitar de um problema

1. **Não tente esconder.** Problemas de segurança escondidos são piores que problemas resolvidos.
2. **Documente o que você encontrou.** Quando, onde, qual a natureza do problema.
3. **Avise imediatamente** quem é responsável pelo sistema.
4. **Desative/bloqueie** o ponto de acesso comprometido enquanto o problema é resolvido.
5. **Não exclua logs** — eles são evidência para entender o impacto.

## Checklist de segurança pré-deploy

- [ ] Nenhuma credencial no código (busque por `sk-`, `pk-`, `password =`, `secret =`)
- [ ] Variáveis de ambiente configuradas corretamente no ambiente de produção
- [ ] RLS ativado no Supabase para todas as tabelas relevantes
- [ ] Autenticação verificada nos endpoints que precisam de login
- [ ] Nenhum dado real de cliente usado em desenvolvimento/teste
- [ ] Dependências verificadas (sem pacotes desconhecidos)
- [ ] Logs de erro não expõem informações internas

## Ferramentas para verificar

```bash
# Procura por possíveis chaves hardcoded no código
grep -r "sk-\|pk-\|password\s*=\|secret\s*=" --include="*.ts" --include="*.tsx" --include="*.js" .

# Verifica se .env está no .gitignore
cat .gitignore | grep .env
```

Se `.env` não está no `.gitignore`, adicione antes de qualquer commit:

```bash
echo ".env" >> .gitignore
echo ".env.local" >> .gitignore
```
