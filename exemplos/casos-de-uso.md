# Exemplos: Casos de Uso no Trabalho

Situações reais onde IA agrega valor, com o nível de cuidado necessário para cada uma.

---

## Nível de cuidado

| Símbolo | Significado |
|---------|-------------|
| 🟢 | Baixo risco — use livremente, revise brevemente |
| 🟡 | Risco médio — revise com atenção antes de usar |
| 🔴 | Alto risco — revisão obrigatória, não vá para produção sem validação do QA |

---

## 🟢 Geração de boilerplate

**Situação:** Você precisa criar o décimo componente de formulário do projeto.

**Como usar:**
```
Cria um componente FormField em src/components/ui/FormField.tsx seguindo
exatamente o mesmo padrão do componente FormInput.tsx que já existe na
mesma pasta. Props: label (string), error (string | undefined), children (ReactNode).
```

**Nível de revisão:** Leia para confirmar que seguiu o padrão. Tempo economizado: 10-15 minutos.

---

## 🟢 Explicação de código legado

**Situação:** Você precisa mexer num arquivo que ninguém lembra como funciona.

**Como usar:**
```
Explique o que essa função faz e por que o código está estruturado assim.
Identifique qualquer comportamento não óbvio ou potencialmente problemático:

[cole a função]
```

**Nível de revisão:** Use como ponto de partida para sua compreensão — verifique se a explicação faz sentido com o que você observa no sistema. Tempo economizado: 20-30 minutos de arqueologia de código.

---

## 🟢 Conversão entre formatos

**Situação:** Você tem um objeto TypeScript e precisa criar o schema equivalente em SQL, ou vice-versa.

**Como usar:**
```
Converte essa interface TypeScript para uma instrução CREATE TABLE no PostgreSQL.
Use snake_case para os nomes das colunas. Adicione created_at e updated_at
com DEFAULT now().

interface Pedido {
  id: string
  clienteId: string
  valorTotal: number
  status: 'pendente' | 'aprovado' | 'cancelado'
  itens: PedidoItem[]
}

Nota: itens é um array que vai em tabela separada, não em JSONB.
```

**Nível de revisão:** Confirme os tipos de dados e que nomes fazem sentido. Tempo economizado: 5-10 minutos.

---

## 🟡 Criação de telas completas

**Situação:** Você precisa criar a tela de configurações do usuário.

**Como usar:** Siga o [`templates/pedido-feature.md`](../templates/pedido-feature.md) completo. Seja específico sobre componentes existentes, dados e comportamentos.

**Nível de revisão:**
- Leia o código gerado completamente
- Teste todos os estados: carregando, erro, sucesso, campos vazios
- Verifique se usa os componentes corretos do design system
- Confirme que os dados são salvos corretamente

Tempo economizado: 2-4 horas, mas invista 30-45 minutos de revisão.

---

## 🟡 Refatoração de código existente

**Situação:** Uma função ficou grande demais e precisa ser quebrada.

**Como usar:**
```
Refatora a função processarPedido em src/services/pedidos.ts.
Ela está com 120 linhas fazendo 4 coisas distintas.

Quero que você:
1. Extraia a validação para validatePedido (linhas 10-35)
2. Extraia o cálculo de preços para calcularTotais (linhas 40-70)
3. Mantenha o resto em processarPedido, que agora chama as duas anteriores

Não altere a interface pública da função (assinatura, o que retorna).
Não altere outros arquivos.
```

**Nível de revisão:** Execute os testes existentes. Se não tiver testes, teste manualmente o fluxo completo. Leia para confirmar que a lógica não mudou. Tempo economizado: 1-2 horas, mas revise com cuidado.

---

## 🟡 Escrita de testes

**Situação:** Você precisa criar testes para uma função existente.

**Como usar:**
```
Escreve testes unitários para a função calcularDesconto em src/utils/precos.ts.

A função recebe (valor: number, percentual: number) e retorna o valor do desconto.

Casos que precisam de teste:
- Desconto normal (valor=100, percentual=10 → retorna 10)
- Percentual zero → retorna 0
- Valor zero → retorna 0
- Percentual acima de 100 → deve lançar erro
- Valores negativos → deve lançar erro

Framework de testes: Vitest. Arquivo de testes em src/utils/precos.test.ts.
```

**Nível de revisão:** Execute os testes e confirme que passam. Verifique se os casos de erro realmente testam o comportamento correto. Tempo economizado: 30-60 minutos.

---

## 🔴 Autenticação e autorização

**Situação:** Implementar login, controle de acesso, permissões.

**Como usar:** Use com contexto extremamente detalhado sobre seu modelo de segurança. Inclua quais usuários podem fazer o quê, como as permissões são estruturadas, quais dados são sensíveis.

**Nível de revisão (responsabilidade do QA):**
- Leia **todo** o código gerado
- Teste tentativas de acesso não autorizado (acesse com usuário B o que pertence ao usuário A)
- Verifique RLS no Supabase se aplicável
- Confirme que tokens/sessões expiram corretamente

Não vá para produção sem validação completa do QA.

---

## 🔴 Operações com dados em produção

**Situação:** Migração de dados, scripts de atualização em massa, limpeza de banco.

**Como usar:** Gere o script, mas **nunca execute direto em produção**.

Processo correto:
1. Gere o script
2. Execute em uma cópia do banco de produção (staging)
3. Verifique os resultados
4. Faça backup do banco de produção
5. Execute em produção
6. Verifique novamente

**Nível de revisão:** Máximo. Operações destrutivas não têm desfazer.

---

## 🔴 Integração com APIs externas de pagamento

**Situação:** Stripe, PagSeguro, MercadoPago, etc.

**Como usar:** Use a IA para entender como a API funciona e gerar a estrutura. Mas leia a documentação oficial da API antes de implementar.

**Nível de revisão:**
- Confirme que webhooks estão sendo validados (evita fraude)
- Confirme que chaves são carregadas de variáveis de ambiente
- Teste o fluxo completo em sandbox antes de produção
- Confirme o tratamento de erros (o que acontece se o pagamento falhar?)

---

## Como decidir o nível de cuidado

Pergunte-se:
- **Isso mexe com dinheiro?** → 🔴
- **Isso mexe com dados de outros usuários?** → 🔴
- **Isso é autenticação ou autorização?** → 🔴
- **Isso mexe com dados em produção?** → 🔴
- **Isso é uma feature nova com lógica de negócio?** → 🟡
- **Isso pode ser desfeito facilmente se der errado?** → 🟢
