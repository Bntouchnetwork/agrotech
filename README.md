# AgroTech — MVP (protótipo)

Protótipo front-end da plataforma **AgroTech**: gestão agrícola e pecuária, Cerca Virtual,
Frigoríficos, Marketplace (incluindo venda de gado em todas as fases) e mais.

É um **aplicativo de página única**, 100% no navegador (React + Babel embutidos no próprio
arquivo). Não precisa de servidor, banco de dados nem build. Todos os dados são de
demonstração e ficam em memória (recarregar a página zera as alterações).

---

## Como publicar no GitHub Pages (passo a passo)

1. Crie um repositório novo no GitHub (pode ser público). Ex.: `agrotech-mvp`.
2. Envie para o repositório os arquivos **`index.html`** e **`.nojekyll`**
   (o `README.md` é opcional).
   - Pela web: botão **Add file → Upload files**, arraste os arquivos e confirme com **Commit changes**.
3. No repositório, vá em **Settings → Pages**.
4. Em **Build and deployment → Source**, escolha **Deploy from a branch**.
5. Em **Branch**, selecione **main** e a pasta **/ (root)**. Clique em **Save**.
6. Aguarde ~1 minuto. A página ficará disponível em:
   `https://SEU-USUARIO.github.io/agrotech-mvp/`

> O arquivo `.nojekyll` faz o GitHub servir o site sem o processamento do Jekyll — recomendado.

---

## Como testar o MVP

1. Abra a URL publicada.
2. Clique em **ENTRAR** → escolha um perfil:
   - **Membro Gold** (pecuarista): usa Gestão Pecuária, Cerca Virtual, Frigoríficos,
     Marketplace (venda de gado), etc.
   - **Admin**: gerencia Frigoríficos, a frota da Cerca Virtual, usuários, etc.
3. Clique em **ENTRAR NO PAINEL** e navegue pelo menu lateral.

Destaques para demonstrar:
- **Gestão Pecuária → Confinamento**: dashboard executivo, lotes, nutrição, água, sanidade.
- **Cerca Virtual**: mapa, cerca virtual, piquetes, mover rebanho, heatmap, alertas,
  automação (SE→ENTÃO), analytics, relatórios, IA e saúde por comportamento.
- **Frigoríficos**: o pecuarista compara preços e vê quem paga mais; o admin cadastra/edita.
- **Marketplace → Gado**: anúncios em todas as fases (cria, recria, engorda, terminação,
  reprodução, leite) com filtros e nº de lote.

---

## Observações

- Funciona em desktop e celular.
- Único acesso externo: autocomplete de endereço (OpenStreetMap) em alguns campos de busca;
  se estiver offline, o restante do app continua funcionando normalmente.
- Por ser um protótipo, preços, telemetria e integrações são simulados (dados em memória).
