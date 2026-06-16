# E-Parceiro

MVP funcional para gestão de serviços elétricos residenciais e prediais. O app cobre abertura de solicitação, triagem por IA, orçamento preliminar, painel técnico, encerramento, relatório e exportação PDF.

## Stack

- Next.js 15 com App Router
- TypeScript
- Tailwind CSS com dark mode
- Supabase Auth, Database e Storage
- Prisma ORM
- OpenAI API com fallback simulado
- Deploy ready para Vercel

## Funcionalidades entregues

- Cliente: cadastro/login Supabase, cadastro demo, abertura de solicitação, descrição, upload de até 5 imagens, status e orçamento preliminar.
- Técnico: dashboard de solicitações abertas, detalhes, imagens, edição/aprovação de orçamento, materiais sugeridos, encerramento e fotos finais.
- IA: categoria, complexidade, materiais, duração, resumo técnico e custo de mão de obra.
- Administrador: métricas básicas, usuários, técnicos, ordens e valores previstos.
- Relatório: geração automática ao concluir, materiais, cliente, técnico, resumo e PDF.
- Banco: `Users`, `Clients`, `Technicians`, `ServiceRequests`, `Budgets`, `Materials`, `ServiceReports`, `Attachments`.

## Setup local

1. Instale dependências:

```bash
npm install
```

No PowerShell com execução de scripts bloqueada, use:

```powershell
npm.cmd install
```

2. Copie as variáveis:

```bash
cp .env.example .env
```

3. Configure `.env`:

```env
DATABASE_URL="postgresql://postgres:password@db.project-ref.supabase.co:5432/postgres?schema=public"
NEXT_PUBLIC_SUPABASE_URL="https://project-ref.supabase.co"
NEXT_PUBLIC_SUPABASE_ANON_KEY="your-anon-key"
SUPABASE_SERVICE_ROLE_KEY="your-service-role-key"
OPENAI_API_KEY=""
NEXT_PUBLIC_APP_URL="http://localhost:3000"
```

4. Crie as tabelas e rode o seed:

```bash
npm run db:push
npm run db:seed
```

5. Inicie:

```bash
npm run dev
```

Acesse `http://localhost:3000`.

## Supabase

Crie um projeto Supabase e copie:

- Project URL para `NEXT_PUBLIC_SUPABASE_URL`
- anon public key para `NEXT_PUBLIC_SUPABASE_ANON_KEY`
- service role key para `SUPABASE_SERVICE_ROLE_KEY`
- connection string PostgreSQL para `DATABASE_URL`

Para Storage, crie um bucket público chamado:

```text
service-attachments
```

O upload de imagens usa `/api/upload`. Se o bucket ainda não existir, o formulário permite colar URLs manualmente para manter o MVP utilizável.

## OpenAI

Defina `OPENAI_API_KEY` para ativar análise real. Sem a chave, `lib/ai.ts` usa uma heurística local que simula a classificação, materiais, duração, resumo e orçamento.

## Dados demo

O seed cria:

- `admin@eparceiro.com`
- `tecnico@eparceiro.com`
- `cliente@eparceiro.com`
- Uma solicitação `EP-0001` com orçamento e materiais.

Esses usuários são registros locais do MVP. Para login real, crie usuários no Supabase Auth pela tela de cadastro.

## Rotas principais

- `/` visão inicial
- `/login` login Supabase
- `/cadastro` cadastro Supabase e usuário demo
- `/cliente` dashboard do cliente
- `/cliente/solicitar` nova solicitação
- `/tecnico` dashboard técnico
- `/admin` dashboard admin
- `/solicitacoes/[id]` detalhe, orçamento e encerramento
- `/relatorios/[id]` relatório concluído
- `/api/service-requests` listagem/criação JSON
- `/api/ai/analyze` análise IA JSON
- `/api/upload` upload Supabase Storage
- `/api/reports/[id]/pdf` exportação PDF

## Deploy Vercel

1. Suba o projeto para um repositório Git.
2. Importe o repositório na Vercel.
3. Configure as variáveis de ambiente na Vercel.
4. Garanta que o banco Supabase esteja acessível pela `DATABASE_URL`.
5. Rode localmente ou via CI:

```bash
npm run db:push
npm run db:seed
```

6. Faça deploy. O script `build` executa `prisma generate` antes de `next build`.

## Observações do MVP

- Upload real depende do bucket público `service-attachments`.
- A autenticação Supabase está pronta no cliente; o MVP também oferece criação demo local para testes rápidos de fluxo.
- O PDF é gerado server-side por uma função leve em `lib/pdf.ts`.
