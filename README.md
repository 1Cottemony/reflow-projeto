# reflow

Guia interativo de responsividade feito em **Vue 3 + Vite**. Cada seção tem explicação curta, exemplo de código comentado, pergunta-guia respondida e fontes. O guia tem laboratórios em que a pessoa mexe em controles e vê o conceito acontecer.

- **Site publicado:** _colar aqui o link do Vercel_
- **Dupla:** _nome 1 e nome 2_

## Seções (uma rota por seção)

| Rota | Conteúdo | Laboratório |
|---|---|---|
| `/` | Home com demonstração interativa | Miniatura que se reorganiza |
| `/por-que` | Por que responsividade; meta viewport | Simulador de largura de tela |
| `/unidades` | px, rem, em, %, fr, vw, vh, dvh, ch | Conversor de unidades |
| `/flex-grid` | Flexbox e Grid, `auto-fit` + `minmax` | Flexbox e Grid ao vivo |
| `/imagens` | max-width, width/height, lazy, aspect-ratio, object-fit, srcset | Imagem que empurra o texto (CLS) |
| `/boas-praticas` | Pastas, props, `:key`, tokens, Core Web Vitals, rotas sob demanda | Lista com e sem `:key` |
| `/desafio` | Pergunta que só se responde com o guia | Prova real com grid medido no navegador |

Além disso, um indicador fixo no canto da tela mostra a largura atual da janela e o breakpoint ativo.

## Tecnologias

Vue 3 (Composition API, `<script setup>`), Vite, Vue Router, Pinia, CSS puro (Grid, Flexbox, variáveis CSS, container queries). Fontes self-hosted via `@fontsource-variable` (Bricolage Grotesque e JetBrains Mono). Nenhuma biblioteca de componentes ou de CSS.

## Como rodar no computador

Requisito: **Node.js 22.18 ou superior** (veja com `node -v`).

```bash
npm install      # instala as dependências (uma vez)
npm run dev      # abre o servidor de desenvolvimento (normalmente http://localhost:5173)
npm run build    # gera a versão de produção na pasta dist/
npm run preview  # abre a versão de produção para testar
```

## Estrutura de pastas

```
src/
  assets/       main.css (design tokens e estilos base) e labs.css (controles dos laboratórios)
  components/
    layout/     SiteHeader, SectionLayout, ViewportRuler, ViewportBadge
    labs/       LabShell, ViewportLab, UnitsLab, FlexGridLab, LayoutShiftLab, ShiftPane, KeyDemo
    content/    CodeBlock, GuideQuestion, SourceList
    challenge/  GridChallenge, GridProbe
    home/       HeroDemo
  config/       breakpoints.js
  data/         sections.js (fonte única das seções)
  router/       index.js (rotas com carregamento sob demanda)
  stores/       viewport.js (Pinia: largura da janela e breakpoint)
  views/        uma página por rota
```

## Decisões de CSS

- **Mobile-first:** o CSS fora de media query é o do celular; todas as media queries usam `min-width`. Breakpoints: 37.5rem (600px), 56.25rem (900px) e 75rem (1200px).
- **Tokens:** cores, fontes e espaçamentos são variáveis CSS em `:root` (`src/assets/main.css`).
- **rem** para fontes e espaçamentos. O tamanho do `<html>` não é fixado, para respeitar a preferência de fonte da pessoa.
- **Sem rolagem horizontal:** tabelas, código e demonstrações largas rolam dentro da própria caixa.
- **Menu:** no celular vira o botão "Menu" (fecha com Esc e ao trocar de página); a partir de 900px os links ficam na barra.
- **Laboratórios** usam `@container` (também `min-width`) porque a moldura de demonstração é uma caixa dentro da página, não a janela.
- Respeita `prefers-reduced-motion`.

## Publicar (Vercel)

1. **Subir para o GitHub.** Crie um repositório vazio em github.com (botão "New repository", sem README). Na pasta do projeto:
   ```bash
   git init
   git add .
   git commit -m "Primeira versão do guia reflow"
   git branch -M main
   git remote add origin https://github.com/SEU-USUARIO/reflow.git
   git push -u origin main
   ```
   Adicione o colega em Settings → Collaborators.
2. **Publicar.** Em vercel.com, entre com a conta do GitHub, clique em **Add New → Project**, escolha o repositório e clique em **Deploy**. O Vercel reconhece o Vite sozinho (build `npm run build`, saída `dist`).
3. **Se o build falhar por versão do Node:** no projeto do Vercel, Settings → General → Node.js Version → escolha 22.x e faça um novo deploy.
4. **Conferir.** Abra o link no celular, navegue pelas seções e, **atualizando a página dentro de `/unidades`**, confira que não aparece erro 404 (isso é o que o `vercel.json` resolve).
5. Cole o link do site no topo deste README e faça `git add . && git commit -m "Link do site" && git push`.

Cada `git push` publica uma nova versão automaticamente.

## Como foi verificado

O build (`npm run build`) passa sem erros. Em Chrome headless foram testadas 8 rotas em 9 larguras (320, 360, 375, 390, 430, 768, 1024, 1280 e 1440px): sem rolagem horizontal, sem conteúdo vazando e sem erros no console. Menu, laboratórios e Desafio foram exercitados por script. **Ainda não foi testado em celular físico** nem em Safari/Firefox.
