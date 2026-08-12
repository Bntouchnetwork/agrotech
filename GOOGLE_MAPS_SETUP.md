# AgroTech Virtual Fence — Integração Google Maps (Etapa 1)

Esta etapa substitui **apenas** o mapa fictício da aba **Mapa** (dentro de
*Cerca Virtual*) pelo **Google Maps real**. Todo o restante do design foi
preservado. GPS de animais, polígonos de piquetes, cercas, heatmap e
geofencing **não** foram implementados ainda — a arquitetura só foi preparada.

---

## 1. Arquivos modificados / criados

| Arquivo | O que é |
|---|---|
| `index.html` | **modificado** — carrega `config.js` no `<head>`; adiciona o componente modular `VFGoogleMap` + o loader `AgroMaps`; na aba Mapa, troca o mapa fictício pelo Google Maps; CSS do mapa; modo inicial `hybrid`. |
| `config.js` | **criado (NÃO versionar)** — onde você insere a `GOOGLE_MAPS_API_KEY`. Está no `.gitignore`. |
| `config.example.js` | **criado (versionar)** — modelo sem a chave. |
| `.gitignore` | **criado** — ignora `config.js`, `.env*` e backups. |
| `.github/workflows/deploy.yml` | **criado (opcional)** — gera `config.js` a partir de um GitHub Secret no deploy. |
| `GOOGLE_MAPS_SETUP.md` | este documento. |

O `mapId` (`f651f614f590ba96d783db3e`) já está embutido no `config.js`/exemplo.

---

## 2. Como a API Key foi configurada (e por quê)

O projeto é uma **SPA de arquivo único** (`index.html` com React embutido inline),
servida **estaticamente** pelo GitHub Pages. **Não há bundler** (Vite/Next/Webpack),
portanto variáveis como `VITE_...` ou `NEXT_PUBLIC_...` **não se aplicam** — não
existe passo de build que as injete.

Num front-end estático, a Maps JavaScript API **precisa** que a chave chegue ao
navegador; não há como "ocultá-la" de fato. A proteção correta e recomendada pelo
Google é a **restrição por HTTP Referrer** (que você já configurou). Por isso:

- A chave fica em **`config.js`** (um único ponto), lido em `window.AGROTECH_CONFIG`.
- `config.js` está no **`.gitignore`** → **não é commitado**.
- `config.example.js` (sem chave) é versionado como modelo.
- Opcionalmente, o **GitHub Actions** gera o `config.js` a partir de um **Secret**
  no deploy, para você nem precisar manter o arquivo localmente no servidor.

> A chave **não** está hardcoded no `index.html`. O único lugar com a chave real é
> o `config.js` (ignorado pelo Git) ou o Secret do GitHub.

---

## 3. Como testar localmente

1. Crie o `config.js` a partir do modelo e insira sua chave:
   ```bash
   cp config.example.js config.js
   # edite config.js e cole a chave em GOOGLE_MAPS_API_KEY
   ```
2. Sirva a pasta por HTTP (a Maps API não roda bem via `file://`):
   ```bash
   python3 -m http.server 5173
   # abra http://localhost:5173/
   ```
3. No app: **ENTRAR → Membro Gold → ENTRAR NO PAINEL → Cerca Virtual → aba Mapa**.
   O mapa deve abrir em **Híbrido**. Use os botões **Satélite / Híbrido / Terreno**.

> Para a chave funcionar em `localhost`, inclua `http://localhost:5173/*`
> (e `http://localhost/*`) nas **restrições de HTTP Referrer** da chave no
> Google Cloud — ou use uma chave separada de desenvolvimento.

Sem chave configurada, a aba Mapa mostra um aviso amigável (não quebra o app).

---

## 4. Como publicar no GitHub Pages

O site está em `https://bntouchnetwork.github.io/agrotech/` (base `/agrotech/`).
O `config.js` é referenciado por **caminho relativo**, então funciona nessa base
sem ajustes.

### Opção A — Simples (Deploy from a branch), sem expor a chave no Git
1. Suba normalmente o `index.html` (e os demais arquivos) para o repositório.
   **Não** suba o `config.js` (o `.gitignore` já o bloqueia).
2. Gere o `config.js` **apenas no ambiente publicado**. Como o Pages serve o que
   está no branch, aqui você tem duas saídas:
   - **A1:** use a **Opção B** (Actions) — recomendada; ou
   - **A2:** mantenha um `config.js` com a chave **somente** se o repositório for
     **privado**. Em repositório público, prefira a Opção B.

### Opção B — GitHub Actions + Secret (recomendada, chave fora do Git)
1. Em **Settings → Secrets and variables → Actions → New repository secret**:
   - Name: `GOOGLE_MAPS_API_KEY`
   - Value: *sua chave*
2. Em **Settings → Pages → Source**, selecione **GitHub Actions**.
3. Faça `push` na branch `main`. O workflow `.github/workflows/deploy.yml`
   gera o `config.js` a partir do Secret e publica. A chave **não** é commitada.

---

## 5. O que você ainda precisa fazer manualmente

1. **Inserir a chave**: em `config.js` (local) **ou** no Secret `GOOGLE_MAPS_API_KEY`
   (para a Opção B).
2. **Google Cloud** — confirme na chave:
   - **Maps JavaScript API** habilitada.
   - **Restrições de HTTP Referrer** incluindo:
     - `https://bntouchnetwork.github.io/agrotech/*`
     - (dev) `http://localhost:5173/*` e `http://localhost/*`
3. **Map ID** `f651f614f590ba96d783db3e` — mantenha-o associado à mesma conta/projeto
   da chave.
4. (Opção B) Definir **Pages → Source = GitHub Actions** uma vez.

---

## 6. Arquitetura (preparada para as próximas etapas)

- `AgroMaps.load()` — carrega o SDK uma única vez (`libraries=maps,marker`, já pronto
  para **Advanced Markers**).
- `VFGoogleMap({ center, zoom, mapType, onReady })` — o callback **`onReady(map, gmaps)`**
  é o gancho para plotar, nas próximas fases: **piquetes (polígonos)**, **cercas
  virtuais**, **posição GPS dos animais**, **advanced markers**, **tempo real**,
  **histórico de deslocamento**, **heatmaps**, **geofencing** e **edição sobre o mapa**.
- Nada disso está ativo ainda — esta etapa entrega **apenas o mapa base** validado.
