# 04 — Como Revisar Código Gerado por IA

Código gerado por IA não vai para produção sem revisão. Ponto. Este guia ensina o que verificar e como fazer essa revisão de forma eficiente.

## O fluxo padrão deve ser:

Envio para validação com o QA (Eu) - Promoção para produção apenas após validação

## A etapa de QA é obrigatória para garantir que:

- O comportamento da funcionalidade está correto
- Não há regressões em funcionalidades existentes
- Casos de erro foram tratados adequadamente
- Não existem riscos de segurança ou exposição de dados
- Mesmo alterações consideradas pequenas devem seguir esse fluxo.

## Resumo do processo:

## Revisão → Teste → QA → Produção

Caso haja necessidade excepcional de pular etapas, a decisão deve ser ***registrada e justificada.***

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
