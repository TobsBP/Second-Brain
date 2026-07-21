Levantado via GitHub API (`Mulher-em-Forma/mef-web-apps-hub`) em 2026-07-21.

## README
"Aplicação Next.js 16 projetada para servir como um hub de soluções externas e ferramentas interativas, como calculadoras de macronutrientes e integração com agendamentos via Calendly."

### Arquitetura (diagrama mermaid no README)
```
Client: Mobile
Web Apps Hub:
  Middleware/Proxy (Auth & i18n Routing)
  Modules: Calculator (WarBaby), Calendly, Admin (Dashboard)
  API: PDF Proxy (/api/pdf)
External: Calendly.com (Embed), PDFHost
```
