# Padrões de Qualidade para Código Gerado por IA

Esses são os critérios mínimos que todo código gerado por IA deve atender antes de ser commitado.

## Legibilidade

- Nomes de variáveis e funções descrevem o que fazem (sem `data`, `temp`, `x`)
- Funções fazem uma coisa só — se a descrição usa "e", considere dividir
- Não tem código comentado — código morto é deletado, não comentado
- Nenhum `console.log` de debug esquecido

## Corretude

- O código faz o que foi pedido (teste manualmente)
- Casos de erro estão tratados (não só o caminho feliz)
- Estados de loading existem para operações assíncronas
- Formulários validam input antes de enviar

## Consistência com o projeto

- Segue o padrão de nomenclatura do projeto (camelCase, PascalCase, etc.)
- Usa os componentes existentes em vez de criar novos equivalentes
- Importações seguem o mesmo padrão do restante do projeto
- Não adiciona dependências não autorizadas

## TypeScript (se aplicável)

- Sem `any` explícito sem justificativa
- Interfaces e tipos definidos para dados de API/banco
- Props de componentes com tipos explícitos

## O que não é negociável

Código que contém qualquer um dos itens abaixo **não é commitado**:

- Credenciais, chaves de API, ou senhas no código
- Dados reais de usuários/clientes em arquivos de teste ou exemplo
- Queries SQL sem filtro de usuário em tabelas de dados pessoais
- Endpoints sem autenticação que deveriam ter
