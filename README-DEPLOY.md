# Publicar a landing page (Apostila Curitiba 2026) — passo a passo

Site estático: um `index.html` + `assets/`. Não tem build, não tem dependência.

## 1. Testar localmente
Dê duplo clique no `index.html` (abre no navegador) e confira a página.
Para testar o link do WhatsApp depois, o site precisa estar publicado (passo 5).

## 2. Subir no GitHub
1. Crie um repositório vazio no GitHub: nome `apostila-curitiba`, **PUBLIC**, sem README.
2. No terminal (dentro desta pasta):
   ```
   git init -b main
   git add index.html assets README-DEPLOY.md
   git commit -m "apostila curitiba 2026 - landing"
   git remote add origin https://github.com/dansoberanis-dev/apostila-curitiba.git
   git push -u origin main
   ```
   (A pasta `deploy/` fica fora do repositório: é ferramenta de trabalho local.)

## 3. Publicar na Vercel
1. vercel.com → **Add New → Project** → importe `apostila-curitiba`.
2. Framework: **Other/estático**, sem build → **Deploy**.
3. Anote a URL (deve ser `https://apostila-curitiba.vercel.app`).

## 4. Cadastrar o produto na Kiwify
Use a URL da Vercel como o "site" do produto. (O botão de compra da página
ainda aponta para um placeholder — ele é substituído pelo link da Kiwify.)

## 5. Gravar a URL final
```
python deploy/preparar.py https://apostila-curitiba.vercel.app
git add index.html robots.txt sitemap.xml
git commit -m "url final no canonical e og"
git push
```
Isso escreve a URL no canonical, nas imagens de compartilhamento (og:image) e
nos dados estruturados, e gera `robots.txt` + `sitemap.xml`.
**Sem isso, o link compartilhado no WhatsApp aparece sem imagem.**

## 6. Substituir o link da Kiwify
O link de compra está no topo do `<script>` do `index.html`:
```js
var URL_COMPRA = "__KIWIIFY__";
```
Troque `__KIWIIFY__` pelo link da Kiwify, depois:
```
git add index.html; git commit -m "link kiwify"; git push
```
Até essa substituição, os botões levam para a seção de oferta da própria página
(comportamento de segurança — nunca um link quebrado).

## Checklist final
- [ ] Abrir a URL no celular (4G) e no computador
- [ ] Colar o link no WhatsApp: deve aparecer cartão com imagem, título e descrição
- [ ] Contagem regressiva mostrando os dias certos até 13/12/2026
- [ ] Botão de comprar abrindo a Kiwify
- [ ] Conferir os dados do edital (250 vagas + CR · R$ 3.027,73 · 13/12/2026 · FAFIPA)

## O que tem no pacote
```
index.html            a página inteira (HTML + CSS + JS inline)
assets/og-image.jpg   imagem de compartilhamento (WhatsApp/Instagram)
deploy/preparar.py    grava a URL final e gera robots.txt/sitemap.xml
README-DEPLOY.md      este guia
```
