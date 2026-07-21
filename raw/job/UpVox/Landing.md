Levantamento de `profissao_laser/landing/`.

## O que é
Mini-projeto isolado dentro do repo `profissao_laser`: uma landing page de marketing ("Comunidade Profissão Laser — O ecossistema completo"), construída **fora** do app Next.js principal.

## Formato
- `Profissao Laser Landing.html` e `profissao-laser-standalone.html` — HTML standalone, carrega Tailwind via CDN (`cdn.tailwindcss.com`) e fontes do Google Fonts diretamente, sem build step.
- `app.jsx` + `components/` (`hero.jsx`, `top-bar.jsx`, `stats-bar.jsx`, `feature-grid.jsx`, `video-section.jsx`, `testimonials.jsx`, `pricing-section.jsx`, `faq-section.jsx`, `final-cta.jsx`, `landing-footer.jsx`) — a mesma landing componentizada em React, renderizada direto num `#app` via `ReactDOM.createRoot` (sem framework/roteador).
- `icons.jsx`, `hooks.jsx` — utilitários locais.

## Por que existe separado
Parece ser um protótipo rápido de landing page — deployável isoladamente (um HTML só) sem depender do pipeline de build do Next.js principal, útil para iterar rápido em copy/design de marketing.
