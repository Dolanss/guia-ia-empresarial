# Exemplos: Prompts que Funcionam

Coleção de prompts reais que geram resultados consistentemente bons. Use como referência ou adapte para seu caso.

---

## Criar componente seguindo padrão existente

```
Cria um componente React chamado UserAvatar em src/components/ui/UserAvatar.tsx.

Siga exatamente o mesmo padrão do componente Button em src/components/ui/Button.tsx
(sem precisar criar variantes, só o caso de uso simples).

O componente deve:
- Receber props: src (string, URL da imagem), name (string, para alt e fallback),
  size (sm | md | lg, default md)
- Se src estiver definido: mostrar a imagem com alt={name}
- Se src não estiver definido: mostrar as iniciais do name em um círculo colorido
- Tamanhos: sm=24px, md=32px, lg=48px

TypeScript strict, sem any. Apenas Tailwind para estilo.
Sem comentários desnecessários no código.
```

**Por que funciona:** referencia código existente, especifica props com tipos, define comportamento para cada caso, lista restrições claras.

---

## Conectar form com Supabase

```
Tenho um formulário de criação de tarefa em src/pages/Tarefas.tsx.
O form já existe com os campos (título, descrição, prioridade).
Só falta a função de salvar.

Contexto:
- Cliente Supabase está em src/lib/supabase.ts (importado como { supabase })
- Tabela no Supabase: `tarefas` com colunas (id, titulo, descricao, prioridade,
  user_id, created_at)
- Usuário autenticado disponível via hook useUser() que retorna { user }

O que preciso:
1. Função handleSubmit que salva no Supabase com user_id = user.id
2. Estado de loading enquanto salva (desabilitar o botão)
3. Mensagem de sucesso após salvar e limpar o formulário
4. Mensagem de erro se falhar (mostrar o erro, não esconder)

Modifique apenas a função handleSubmit e o estado relacionado.
Não altere os campos do formulário nem o layout.
```

**Por que funciona:** mostra o que já existe, especifica a estrutura de dados, define o escopo exato da mudança.

---

## Corrigir bug com erro específico

```
Recebo esse erro ao tentar fazer login:

  AuthApiError: Invalid login credentials
  at signInWithPassword (auth.ts:47)

O e-mail e senha são válidos — consigo logar direto no painel do Supabase.
O problema está na chamada do client-side.

Código atual em src/lib/auth.ts:
---
export async function signIn(email: string, password: string) {
  const { data, error } = await supabase.auth.signInWithPassword({
    email,
    password,
  })
  if (error) throw error
  return data
}
---

O .env.local tem VITE_SUPABASE_URL e VITE_SUPABASE_ANON_KEY definidos.
O cliente Supabase em src/lib/supabase.ts usa process.env — suspeito que
o problema seja que Vite usa import.meta.env, não process.env.

Confirme se é isso e corrija apenas o cliente Supabase em src/lib/supabase.ts.
```

**Por que funciona:** erro completo, código relevante, diagnóstico inicial, escopo cirúrgico.

---

## Adicionar validação

```
Adicione validação no formulário de cadastro em src/pages/Cadastro.tsx.

Regras de validação:
- nome: obrigatório, mínimo 2 caracteres
- email: obrigatório, formato válido de e-mail
- senha: obrigatório, mínimo 8 caracteres, pelo menos uma letra maiúscula
  e um número
- confirmacaoSenha: deve ser idêntico ao campo senha

Comportamento:
- Erros aparecem abaixo de cada campo, em vermelho, somente após o usuário
  tentar submeter ou sair do campo (blur)
- Botão "Cadastrar" fica desabilitado enquanto há erros
- Use validação nativa JavaScript, sem instalar biblioteca nova (ex: sem zod,
  sem react-hook-form)

Não altere o layout nem o estilo dos campos — só adicione as mensagens de erro
abaixo de cada um.
```

**Por que funciona:** regras precisas, comportamento UX definido, restrição de dependências explícita.

---

## Refatorar para reutilização

```
Tenho esse trecho de código repetido em 3 arquivos diferentes
(Dashboard.tsx, Relatorios.tsx, Perfil.tsx):

---
const [loading, setLoading] = useState(false)
const [error, setError] = useState<string | null>(null)

async function fetchData() {
  setLoading(true)
  setError(null)
  try {
    // ... lógica específica de cada arquivo
  } catch (err) {
    setError(err instanceof Error ? err.message : 'Erro desconhecido')
  } finally {
    setLoading(false)
  }
}
---

Crie um hook useAsync em src/hooks/useAsync.ts que encapsula esse padrão.

O hook deve:
- Receber uma função async como parâmetro
- Retornar { data, loading, error, execute }
- Chamar execute() para disparar a função
- Funcionar com TypeScript genérico (o tipo do data deve ser inferido)

Depois, mostre como atualizar Dashboard.tsx para usar o hook.
Não mexa nos outros arquivos ainda.
```

**Por que funciona:** mostra o problema concreto, define a interface do hook com precisão, limita o escopo.

---

## Explicar código que você não entende

```
Explique o que esse código faz, em linguagem simples:

---
export function useDebounce<T>(value: T, delay: number): T {
  const [debouncedValue, setDebouncedValue] = useState<T>(value)

  useEffect(() => {
    const handler = setTimeout(() => {
      setDebouncedValue(value)
    }, delay)

    return () => {
      clearTimeout(handler)
    }
  }, [value, delay])

  return debouncedValue
}
---

Explique:
1. O que esse hook faz do ponto de vista do usuário (comportamento observável)
2. Por que o clearTimeout na função de cleanup é importante
3. Um caso de uso prático onde você usaria isso
```

**Por que funciona:** pede explicação em camadas, foca no "por quê" não só no "o quê".
