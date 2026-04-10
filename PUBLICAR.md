# Publicar em https://lorenzo-sarcinelli-pascoal.github.io/dashboard-perpetuo-lang/

O workflow envia a pasta **`docs/`** para a branch **`gh-pages`**. O GitHub Pages serve a partir dessa branch (modelo estável; não depende de “GitHub Actions” como *source* no painel).

## 1. Criar o repositório no GitHub

1. [github.com/new](https://github.com/new) → nome **`dashboard-perpetuo-lang`** → **Public** → sem README inicial.

## 2. Enviar o código (mono-repo `debriefings-the`)

Na **raiz** do repositório que contém `dashboard-perpetuo-lang/`:

```bash
cd /caminho/para/debriefings-the-main
git pull origin main

git branch -D lang-github-pages 2>/dev/null
git subtree split -P dashboard-perpetuo-lang -b lang-github-pages

git push git@github.com:lorenzo-sarcinelli-pascoal/dashboard-perpetuo-lang.git lang-github-pages:main --force
```

## 3. Primeiro deploy (Actions)

Abra **Actions** e espere o workflow **Deploy GitHub Pages** concluir. Ele cria/atualiza a branch **`gh-pages`**.

## 4. Ligar o Pages na branch `gh-pages`

No repo **`dashboard-perpetuo-lang`**:

**Settings → Pages → Build and deployment**

- **Source:** **Deploy from a branch**
- **Branch:** **`gh-pages`** / **`/ (root)`**
- Save

## 5. URL

**https://lorenzo-sarcinelli-pascoal.github.io/dashboard-perpetuo-lang/**

---

### Se antes falhou com `configure-pages`

Esse erro vinha do workflow antigo (artifact + “GitHub Actions” como source). O fluxo atual **não usa** `configure-pages`. Faça pull do mono-repo, **subtree push** de novo e configure o Pages como no passo 4.

### Manutenção

Após alterar `dashboard-perpetuo-lang/` no mono-repo: commit em `debriefings-the`, depois de novo `subtree split` + `push` (passo 2).
