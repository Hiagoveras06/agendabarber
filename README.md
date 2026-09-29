# Barbearia — Autenticação com Supabase

## 1. Instalar dependências
```bash
npm install
```

## 2. Configurar o Supabase
No painel do Supabase: **Project Settings → API**.

Copie `.env.example` para `.env.local`:
```bash
cp .env.example .env.local
```

Edite `.env.local`:
```
VITE_SUPABASE_URL=https://SEU-PROJETO.supabase.co
VITE_SUPABASE_PUBLISHABLE_KEY=SUA-CHAVE-PUBLICAVEL-ou-ANON
```
Use a chave **publishable** (ou **anon public**, em projetos mais antigos) — nunca a `service_role`.

## 3. Rodar a migration
No painel do Supabase, vá em **SQL Editor**, cole o conteúdo de
`supabase/migrations/0001_profiles.sql` e execute. Isso cria a tabela
`profiles`, as políticas de RLS e o gatilho que copia nome/telefone
automaticamente para `profiles` sempre que um usuário é criado.

## 4. Rodar o projeto
```bash
npm run dev
```
Abra o endereço mostrado no terminal (normalmente http://localhost:5173).

## 5. Testar o cadastro
1. Acesse `/register`, preencha o formulário e envie.
2. Se a confirmação de e-mail estiver **desativada** (Authentication →
   Providers → Email, no seu projeto Supabase), você entra direto em `/app`.
3. Se estiver **ativada**, a tela mostra "Confirme seu e-mail" — abra o link
   recebido para poder entrar.

## 6. Testar o login
Acesse `/login` com o e-mail e a senha cadastrados. Um erro de credenciais
mostra "E-mail ou senha incorretos" sem detalhes técnicos.

## 7. Conferir no Supabase
- **Authentication → Users**: o novo usuário aparece automaticamente após o
  cadastro.
- **Table Editor → profiles**: a linha correspondente aparece com `nome` e
  `telefone`, com o mesmo `id` do usuário.

## Sessão
A sessão é gerenciada pelo próprio Supabase (`persistSession: true`). Ao
recarregar a página, `/app` continua acessível enquanto a sessão for válida;
sem sessão, qualquer rota privada redireciona para `/login`.

## O que este pacote NÃO inclui
Ele conecta apenas as telas de login, cadastro, recuperação de senha e uma
tela `/app` mínima para validar a sessão. As demais telas do protótipo
(agendamento, agenda do barbeiro, painel administrativo) ainda vivem no
arquivo HTML único publicado anteriormente e precisam ser portadas para cá
como um passo seguinte, quando fizer sentido.
