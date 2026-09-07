# Minha Quitandinha — como colocar no ar e usar com as sócias

Você vai usar dois serviços, os dois gratuitos:

- **Supabase** — o banco de dados. É onde os condomínios ficam guardados de verdade.
- **Vercel** — hospeda a página. É o endereço que vocês três vão abrir no navegador.

Tempo total: cerca de 20 minutos. Você faz uma vez só.

---

## Parte 1 — Criar o banco no Supabase

**1.** Entre em `supabase.com`, clique em *Start your project* e crie a conta (dá para entrar com o GitHub ou com e-mail).

**2.** Clique em *New project*.
- Name: `minha-quitandinha`
- Database Password: crie uma senha forte e **guarde num lugar seguro**
- Region: `South America (São Paulo)`
- Clique em *Create new project* e espere uns 2 minutos.

**3.** No menu da esquerda, abra **SQL Editor** → *New query*. Cole todo o conteúdo do arquivo `supabase.sql` e clique em **Run**. Deve aparecer *Success*.

**4.** Ainda no menu esquerdo, vá em **Authentication → Users → Add user → Create new user**. Crie **três usuários**, um para cada uma:
- e-mail e senha de cada sócia
- marque a opção **Auto Confirm User** (senão o Supabase manda e-mail de confirmação)

**5.** Vá em **Project Settings → API** e copie duas informações:
- **Project URL** — algo como `https://abcdefgh.supabase.co`
- **anon public** key — um texto longo começando com `eyJ...`

> A chave `anon` pode ficar visível no site. Quem manda na segurança são as regras que o `supabase.sql` criou: sem login, ninguém lê nem escreve nada. **Nunca** use a chave `service_role` no arquivo.

---

## Parte 2 — Colar as chaves no arquivo

Abra o `index.html` num editor de texto (Bloco de Notas serve) e procure, logo no começo do corpo do arquivo:

```js
const SUPABASE_URL  = "";
const SUPABASE_KEY  = "";
```

Preencha com o que você copiou:

```js
const SUPABASE_URL  = "https://abcdefgh.supabase.co";
const SUPABASE_KEY  = "eyJhbGciOi...";
```

Salve. Enquanto essas duas linhas estiverem vazias, o app funciona em modo local (só no seu navegador, sem compartilhar) — útil para testar.

---

## Parte 3 — Publicar na Vercel

O jeito mais rápido, sem GitHub:

**1.** Coloque o `index.html` sozinho numa pasta no computador, por exemplo `minha-quitandinha`.

**2.** Entre em `vercel.com`, crie a conta e vá em **Add New → Project → Deploy from a folder** (ou arraste a pasta na tela inicial).

**3.** Arraste a pasta inteira e clique em **Deploy**. Em menos de um minuto a Vercel devolve um endereço tipo `minha-quitandinha.vercel.app`.

**4.** Abra o endereço, entre com um dos e-mails que você criou no passo 1.4 e mande o link para as sócias.

### Se preferir com GitHub (recomendado a médio prazo)
Suba o `index.html` num repositório, e na Vercel escolha **Import Git Repository**. A vantagem: cada alteração que você salvar no GitHub republica o site sozinho.

Não precisa configurar nada de framework — quando ela perguntar, escolha **Other**, sem build command.

---

## Parte 4 — Deixar tudo pronto para as três

Ao abrir o app pela primeira vez:

1. Clique em **Equipe** e cadastre o nome de cada uma. No campo de e-mail, coloque o **mesmo e-mail do login** — assim o app já reconhece quem está usando e preenche o responsável sozinho ao criar um cartão.
2. A partir daí, o campo *Responsável pelo lead* vira uma lista para escolher, em vez de digitar.

---

## Como funciona o salvamento entre aparelhos

- Quando você clica em **Salvar** dentro de um cartão, a alteração é enviada ao banco na hora.
- O botão **Sincronizar**, no topo, mostra o estado real: *Tudo salvo há X min*.
- Se a internet cair, o botão fica **amarelo** com o número de alterações que ainda não subiram (*Salvar em todos os aparelhos · 2*). Nada é perdido: fica guardado no aparelho, o app tenta reenviar sozinho a cada 30 segundos e você pode forçar clicando no botão.
- Clicar no botão também **puxa** o que as sócias mexeram.
- Com internet, os cartões movidos por uma aparecem para as outras em segundos, sem precisar recarregar a página.
- Se você tentar fechar a aba com algo pendente, o navegador avisa.

---

## Para não perder nada

1. **O Supabase já guarda backup automático** dos últimos dias no plano gratuito.
2. **Exportar CSV**, no topo do quadro, baixa tudo para abrir no Excel. Vale fazer uma vez por semana e guardar no Drive.
3. Todo cartão guarda um **histórico** de quem mexeu e quando, no bloco *Registro de atualizações*.
4. Nunca apague o projeto no Supabase por engano — no plano gratuito, projetos sem nenhum acesso por 7 dias entram em pausa; basta entrar no painel e clicar em *Restore* para voltar.

---

## Problemas comuns

| O que aparece | O que fazer |
|---|---|
| "Sem conexão com o banco" no topo | As chaves estão erradas ou com espaço sobrando. Confira a Parte 2. |
| "E-mail ou senha incorretos" | O usuário não foi criado, ou ficou sem *Auto Confirm*. Refaça o passo 1.4. |
| Entra, mas o quadro fica vazio para uma sócia e cheio para outra | O `supabase.sql` não rodou inteiro. Rode de novo — ele pode ser executado quantas vezes quiser. |
| Cartão da sócia não aparece sozinho | A linha final do `supabase.sql`, do *realtime*, não rodou. Rode-a de novo e recarregue a página. |
