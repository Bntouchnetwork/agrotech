# 📤 AgroTech — O que subir e como configurar a chave (modo SEGURO)

Esta pasta contém **tudo que você deve subir para o GitHub**.
A sua Google Maps API Key **NÃO** está aqui — ela entra por um "Secret" do GitHub,
e o próprio GitHub monta o `config.js` com a chave durante a publicação.

---

## ✅ Arquivos desta pasta (suba TODOS)

    index.html                      → o app
    config.example.js               → modelo (sem chave)
    .gitignore                      → impede subir a chave por engano
    .nojekyll                       → evita processamento indevido do GitHub
    README.md                       → informações gerais
    GOOGLE_MAPS_SETUP.md            → detalhes técnicos
    LEIA-ME-PRIMEIRO.md             → este guia
    .github/workflows/deploy.yml    → publica o site e injeta a chave

> ⚠️ NÃO existe (e não deve existir) um `config.js` aqui. Ele é gerado
> automaticamente pelo GitHub com a sua chave. É isso que mantém a chave segura.

---

## 🚀 Passo a passo (faça na ordem)

### 1) Suba os arquivos para o repositório
- Coloque **todo o conteúdo desta pasta na RAIZ** do repositório do site
  (o mesmo onde já está o site hoje).
- Inclua a pasta oculta **`.github`** (ela traz o `deploy.yml`).

### 2) Cadastre a chave como Secret (aqui vai a sua chave)
No GitHub, dentro do repositório:
- **Settings → Secrets and variables → Actions → New repository secret**
- **Name:** `GOOGLE_MAPS_API_KEY`
- **Secret:** cole a sua chave (ex.: `AIzaSy...`)
- Clique em **Add secret**

### 3) Troque a fonte do Pages para GitHub Actions
- **Settings → Pages → Build and deployment → Source:** selecione **GitHub Actions**
  (em vez de "Deploy from a branch").

### 4) Publique
- Faça um commit/push qualquer na branch principal (`main` ou `master`),
  ou rode o workflow manualmente em **Actions → Deploy AgroTech to GitHub Pages → Run workflow**.
- Em ~1–2 min o site publica **com o mapa já funcionando**.

---

## 🔒 No Google Cloud (uma vez, senão o mapa não carrega)
Na mesma chave, confirme:
- **Maps JavaScript API** habilitada.
- **Restrições → HTTP Referrer** incluindo o endereço do seu site, por exemplo:
  `https://SEU-USUARIO.github.io/SEU-REPO/*`

---

## Como saber se deu certo
Abra o site → **ENTRAR → Membro Gold → ENTRAR NO PAINEL → Cerca Virtual → aba Mapa**.
- Deu certo: aparece o **Google Maps real** (inicia em Híbrido; botões Satélite/Híbrido/Terreno funcionam).
- Falta a chave/Secret: aparece o aviso "Google Maps não configurado" (o resto do app funciona normalmente).
