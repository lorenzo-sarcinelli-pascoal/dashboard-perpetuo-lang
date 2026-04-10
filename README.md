# dashboard-perpetuo-lang

Use esta pasta como **raiz de um repositório Git** separado (ex.: `git init` aqui e push para `github.com/.../dashboard-perpetuo-lang`).

Cópia do dashboard perpétuo com **Google Sheets** (API Key + Spreadsheet ID + nomes das abas), alinhada ao `dashboard-perpetuo.html` do repositório THE (pasta `dashboards/perpetuo/`). Se este for um clone isolado, ignore o link relativo no histórico do mono-repo.

## Conteúdo

| Caminho | Descrição |
|---------|-----------|
| `docs/index.html` | Dashboard (entrada para GitHub Pages) |
| `docs/manifest.webmanifest` | PWA |
| `docs/icons/` | Favicon e ícones THE |

Configuração fica no **localStorage** do navegador (`the_perpetuo_lang_dashboard_config`), separada dos outros dashboards perpétuo.

## Mapeamento da planilha Lang

O motor interno ainda usa os nomes do export Hubla / `metaads_daily`. Antes de `processData`, as linhas passam por `normalizeLangVenda` e `normalizeLangMeta` em [`docs/index.html`](docs/index.html) (comentários no código).

| Canônico (motor) | Aba vendas (Lang) | Notas |
|------------------|-------------------|--------|
| `_netPrice` / receita | `price_brl` (ou `Price_BRL`) | faturamento líquido do dashboard usa esta coluna; se vazia, `_netPrice` = `payment_price` − `commission_plataform` |
| `payment_price` | coluna da planilha | usado no fallback quando `price_brl` está vazio |
| `commision_value_brl` / `comission_value_brl` | (opcional) | podem existir no export; **não** entram mais no cálculo de faturamento líquido do motor |
| `launch_tag` | `launch_tag`, `LaunchTag`, `Launch Tag`, etc. | normalizado em `normalizeLangVenda`; alimenta o filtro **LaunchTag** na barra do painel |
| `approved_date` e `order_date` | `update_time` | só quando ambos canônicos vazios; não usa `create_time` |
| `commission_plataform` | igual | KPI “Taxa plat.” (soma separada) |
| `utm_*`, `status`, `offer_name` | igual | `utm_source` meta/fb/ig → `Meta_Ads` |
| `campaign_id` (match numérico) | `campaign_id` | só esta coluna (10+ dígitos) |

| Canônico (motor) | Aba metaads (Lang) | Notas |
|------------------|---------------------|--------|
| `date` | `data` | sem fallback `id_data` |
| `spend` | `gasto` | — |
| `impressions` | `impressoes` | — |
| `inline_link_clicks` | `clicks` | sem `unique_clicks` |
| `landing_page_view` | `land_page_views` | — |
| `lead` | `leads` | — |
| `purchase` | — | não mapeia `conversions` |
| `campaign_name` | `Campanha` | maiúscula |
| `ad_name` | `ad` | — |
| `adset_name` | `Conjunto` | gasto e “Top conjuntos” por conjunto; match vendas: `utm_medium` × nome do conjunto (método nome) |

## Google Sheets

1. No [Google Cloud Console](https://console.cloud.google.com/), crie um projeto (ou use um existente), ative a **Google Sheets API** e crie uma **API key** (restrita à Sheets, se possível).
2. Compartilhe a planilha (ou torne-a legível conforme sua política) para que a API key possa ler os dados.
3. No dashboard, preencha **API Key**, **Spreadsheet ID** (trecho da URL entre `/d/` e `/edit`), **nome exato da aba de vendas** e **nome exato da aba Meta**.
4. **Salvar e carregar**.

As abas devem ter **cabeçalho na primeira linha** com as colunas esperadas pelo motor do perpétuo (export Hubla + `metaads_daily`).

## GitHub Pages

URL publica: **https://lorenzo-sarcinelli-pascoal.github.io/dashboard-perpetuo-lang/**

1. O código desta pasta deve estar na raiz do repositório GitHub **`lorenzo-sarcinelli-pascoal/dashboard-perpetuo-lang`**.
2. O workflow [`.github/workflows/pages.yml`](.github/workflows/pages.yml) copia `docs/` para a branch **`gh-pages`** ([peaceiris/actions-gh-pages](https://github.com/peaceiris/actions-gh-pages)).
3. **Settings → Pages → Source:** **Deploy from a branch** → **`gh-pages`** / **`/ (root)`** (após o primeiro run do workflow).

Se você mantém o projeto dentro do mono-repo **debriefings-the**, siga **[PUBLICAR.md](PUBLICAR.md)** (subtree split + push).

URL: **https://lorenzo-sarcinelli-pascoal.github.io/dashboard-perpetuo-lang/** (igual padrão de projeto ao [dashboard-perpetuo](https://lorenzo-sarcinelli-pascoal.github.io/dashboard-perpetuo/), pode diferir só o mecanismo de deploy no GitHub).

Teste local: na pasta `docs/`, `python3 -m http.server 8080` e abra `http://localhost:8080/index.html`.

## Ofertas

O mapa padrão de categorias de oferta está vazio; após o primeiro carregamento, classifique cada `offer_name` no painel **Categorização de ofertas**.

## Marca

Ícones: ver `docs/icons/README.md` (Tracking HighEnd).
