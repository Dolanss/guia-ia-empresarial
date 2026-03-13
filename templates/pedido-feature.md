# Template: Pedido de Feature

Use quando quiser pedir uma funcionalidade nova. Preencha antes de escrever o prompt.

---

```
## Feature: [nome curto da funcionalidade]

### O que deve fazer
[descrição clara do comportamento esperado, do ponto de vista do usuário]

Exemplo: "O usuário clica em 'Exportar', escolhe o formato (CSV ou PDF),
e o arquivo é baixado automaticamente."

### Onde fica
- **Tela/página:** [ex: Dashboard, tela de Relatórios]
- **Componente pai:** [ex: dentro do componente RelatorioTable]
- **Arquivos que provavelmente serão tocados:** [se souber]

### Dados envolvidos
- **Dados de entrada:** [o que o usuário fornece]
- **Dados de saída:** [o que é gerado/exibido]
- **Fonte dos dados:** [banco de dados / API externa / estado local]

### Comportamento esperado

**Fluxo principal:**
1. [passo 1]
2. [passo 2]
3. [resultado]

**Casos de erro:**
- Se [condição X]: [o que deve acontecer]
- Se [condição Y]: [o que deve acontecer]

### Restrições
- [ ] Não criar componentes novos — usar os existentes em [pasta]
- [ ] Não instalar dependências novas
- [ ] Manter [comportamento específico] que já existe
- [ ] [outra restrição]

### Critério de pronto
[Como eu vou saber que está funcionando? Ex: "Ao clicar em Exportar com
a tabela populada, um arquivo CSV é baixado com os dados corretos."]

### Formato de output
[só código / código com explicação / explicação primeiro, depois o código]
```

---

## Exemplo preenchido

```
## Feature: Filtro de data no relatório de solicitações

### O que deve fazer
Na tela de Relatórios, o usuário pode filtrar as solicitações por
período. Ele seleciona data de início e data de fim, clica em
"Filtrar", e a tabela mostra apenas as solicitações dentro desse
intervalo.

### Onde fica
- **Tela/página:** /pages/Relatorios.tsx
- **Componente pai:** acima do componente SolicitacoesTable
- **Arquivos que provavelmente serão tocados:** Relatorios.tsx,
  e provavelmente criar um hook useSolicitacoes.ts

### Dados envolvidos
- **Dados de entrada:** data_inicio (string ISO), data_fim (string ISO)
- **Dados de saída:** lista filtrada de solicitações
- **Fonte dos dados:** tabela `solicitacoes` no Supabase, campo `created_at`

### Comportamento esperado

**Fluxo principal:**
1. Usuário vê dois campos de data (início e fim) acima da tabela
2. Seleciona as datas
3. Clica em "Filtrar"
4. Tabela atualiza com resultados filtrados
5. Um label mostra "X resultados encontrados"

**Casos de erro:**
- Se data_fim < data_inicio: mostrar erro "Data final deve ser após a data inicial"
- Se nenhuma solicitação no período: mostrar "Nenhuma solicitação encontrada nesse período"
- Se erro de rede: mostrar mensagem de erro e manter dados anteriores

### Restrições
- Usar os componentes de Input e Button de /components/ui/
- Não criar tabela nova — filtrar na query Supabase com .gte() e .lte()
- Manter a paginação que já existe na tabela

### Critério de pronto
Ao selecionar de 01/01/2025 a 31/01/2025 e filtrar, só aparecem
solicitações criadas em janeiro de 2025.

### Formato de output
Código completo dos arquivos modificados/criados.
```
