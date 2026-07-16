[README (1).md](https://github.com/user-attachments/files/30098147/README.1.md)
# Mulheres que Voam — Sistema de Gestão + Catálogo de Ingressos

Dois arquivos, um só banco de dados (Supabase), no mesmo projeto da fitness — isolado por tabelas próprias:

- **`mulheres-que-voam.html`** — sistema interno (login obrigatório): cadastro de mulheres, eventos, participantes confirmadas, custos, saldo, histórico. É aqui que você marca um evento como "à venda" e define o preço do ingresso.
- **`mv-eventos-catalogo.html`** — vitrine pública (sem login): mostra só os eventos marcados como "à venda" no sistema interno, com botão "Comprar ingresso" que abre o WhatsApp com o pedido pronto.

## 1. Rodar o SQL no Supabase (mesmo projeto da fitness)

1. Abra o **mesmo projeto** Supabase que já usa para a fitness.
2. **SQL Editor → New query** → cole o conteúdo de `supabase-setup-mv-sistema.sql` → **Run**.
3. Isso cria 4 tabelas isoladas (prefixo `mv_`): `mv_mulheres`, `mv_eventos`, `mv_participantes`, `mv_custos` — nenhuma delas toca nas tabelas da fitness.
   - `mv_eventos` tem uma regra especial: **qualquer pessoa** (sem login) pode ver os eventos com `ativo_venda = true` e `status = 'aberto'` — é isso que alimenta o catálogo público. Todo o resto (mulheres, participantes, custos, e a gestão completa de eventos) só é visível pra quem estiver logado.

## 2. Pegar as chaves do Supabase

Em **Project Settings → API** (mesmo projeto): **Project URL** e **anon public key**.

Cole essas duas chaves **nos dois arquivos**:
- `mulheres-que-voam.html` (topo do `<script>`)
- `mv-eventos-catalogo.html` (topo do `<script type="module">`)

## 3. Criar sua conta de acesso

No sistema interno (`mulheres-que-voam.html`), na primeira vez que abrir, vai aparecer a tela de login. Clique em **"criar conta"**, defina e-mail e senha (só sua/da equipe).

## 4. Migrar os dados que já existem no navegador

Se você já usou o sistema antigo (localStorage) e tem dados cadastrados, antes de trocar de arquivo:
1. Abra o **arquivo antigo** no navegador onde os dados estão.
2. Clique em **"Exportar backup"** — isso baixa um `.json` com tudo que você já cadastrou.
3. Abra o **novo** `mulheres-que-voam.html` (já configurado e logado).
4. Clique em **"Importar backup"** e selecione esse `.json`. Ele entra direto no Supabase (mulheres, eventos, participantes e custos), sem apagar nada que já esteja lá.

## 5. Publicar os dois no GitHub Pages

Pode ser em repositórios separados ou no mesmo repositório (cada `.html` fica com seu próprio link):

1. Crie o repositório (ou use um que já tenha criado).
2. Suba os dois arquivos: `mulheres-que-voam.html` (sistema interno) e `mv-eventos-catalogo.html` (catálogo público).
3. **Settings → Pages** → **Deploy from a branch** → `main` → `/ (root)` → **Save**.
4. Os links ficam:
   - Sistema interno: `https://seu-usuario.github.io/repo/mulheres-que-voam.html`
   - Catálogo público: `https://seu-usuario.github.io/repo/mv-eventos-catalogo.html`

> Só o catálogo público deve ser divulgado pra fora — o sistema interno pede login, mas não custa manter o link dele restrito à equipe.

## 6. Ainda falta preencher

- `WHATSAPP_NUMBER` no topo do `mv-eventos-catalogo.html` — o número que recebe os pedidos.

Quando isso estiver testado, seguimos para o bot do WhatsApp que já tínhamos desenhado.
