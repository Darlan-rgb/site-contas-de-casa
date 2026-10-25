# Contas de Casa — versão inicial instalável

Aplicativo web responsivo (PWA) com autenticação Supabase, espaço familiar, carteiras, contas, pagamentos parciais, entradas previstas/recebidas, histórico, relatórios básicos e exportação JSON.

## Requisitos
- Projeto Supabase (plano gratuito pode servir para começar).
- Conta Vercel para publicação estática (ou outro host HTTPS).
- Navegador moderno. Não é necessário publicar na Play Store para instalar no Android.

## 1. Configurar o banco e a autenticação
1. Abra o projeto Supabase.
2. Em **SQL Editor**, crie uma consulta e execute todo o conteúdo de `supabase/schema.sql`.
3. Em **Authentication → URL Configuration**, adicione a URL local/de produção em Site URL e Redirect URLs quando aplicável.
4. Em **Project Settings → API** (ou Data API), copie **Project URL** e a chave pública **anon/publishable**.
5. Edite `config.js` e substitua os dois valores de exemplo. Nunca use `service_role` ou `secret key` no navegador.
6. Em **Authentication → Providers → Email**, confira as configurações de confirmação de e-mail. Para produção, mantenha confirmação de e-mail ativada.

## 2. Testar localmente
Não abra `index.html` diretamente por `file://`. Use um servidor local, por exemplo:

```bash
npx serve .
```

Depois abra a URL exibida pelo servidor. O navegador precisa acessar a internet para carregar o SDK Supabase e autenticar/sincronizar.

## 3. Publicar na Vercel
1. Coloque esta pasta em um repositório Git seu (ou importe o projeto pelo painel da Vercel).
2. Configure o diretório raiz como a pasta que contém `index.html`.
3. Para este projeto estático, não é necessário comando de build; a saída é a própria pasta (`.`). Se o painel pedir, Build Command vazio e Output Directory `.`.
4. Depois de publicar, adicione a URL HTTPS da Vercel em Supabase → Authentication → URL Configuration.
5. Faça login e crie a família. O primeiro usuário será administrador; nas Configurações, compartilhe o código de convite com o segundo usuário. O segundo usuário deve criar a própria conta e entrar com esse código.

## 4. Instalar no Android
Abra a URL publicada no Chrome, menu ⋮ → **Instalar app** ou **Adicionar à tela inicial**. A instalação PWA depende de HTTPS.

## Segurança e limites desta versão
- As chaves públicas Supabase são destinadas ao navegador; o acesso é restringido por RLS e políticas de família. Não coloque segredos no `config.js`.
- Pagamentos e recebimentos são feitos por funções SQL transacionais; há chave de idempotência única no livro-razão e validação de saldo no servidor.
- O schema inicial permite uma família por usuário e um código de convite compartilhado; não é um sistema corporativo de convites com expiração.
- Esta versão ainda não implementa recorrências/parcelamentos automáticos, edição de membros, ajuste de saldo auditado, restauração por importação, transferência entre carteiras, drag-and-drop, notificações externas ou resolução de conflitos offline. O modo offline mantém recursos estáticos em cache, mas **não** grava transações financeiras offline.
- A tela de relatórios é básica; a projeção é saldo atual menos saldo restante das contas cadastradas mais entradas previstas, sem geração automática de ocorrências futuras.
- O histórico e os saldos só refletem corretamente as contas pagas por este app depois da configuração do schema. Não use exemplos de teste como dados reais.
- O SQL deve ser revisado e testado no seu projeto Supabase antes de cadastrar movimentações reais. Esta entrega ainda não foi validada contra um projeto Supabase real, pois não temos acesso às suas credenciais/projeto.

## Arquivos
- `index.html`, `styles.css`, `app.js`: aplicação.
- `config.js`: URL e chave pública do seu projeto.
- `supabase/schema.sql`: tabelas, RLS e funções transacionais.
- `manifest.webmanifest`, `sw.js`, `icon.svg`: suporte PWA.
