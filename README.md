# inspira-templates

5 templates HTML editoriais para os carrosséis Instagram da Inspira Músicas. Servidos via raw.githubusercontent.com e consumidos pelo microsserviço `inspira-render`.

## Estrutura

```
inspira-templates/
├── _base.css                       # Variáveis, classes compartilhadas
├── cover_cinematic.html            # Capa (foto fullscreen + headline)
├── text_top_photo_bottom.html      # Narrativo (creme)
├── text_photo_text.html            # Construção (verde)
├── photo_top_text_bottom.html      # Foto domina (creme)
└── cta_final.html                  # Conversão WhatsApp (pêssego)
```

## Antes de fazer push

**CRITICAL**: Em cada `.html`, substituir `CHANGE-ME` pelo seu username GitHub na linha:

```html
<link rel="stylesheet" href="https://raw.githubusercontent.com/CHANGE-ME/inspira-templates/main/_base.css">
```

Comando rápido:
```bash
sed -i '' 's/CHANGE-ME/seu-usuario-github/g' *.html
```

## Placeholders por template

| Template | Placeholders |
|---|---|
| `cover_cinematic` | `{{image}}`, `{{headline}}`, `{{highlight}}` |
| `text_top_photo_bottom` | `{{text_top}}`, `{{image}}` |
| `text_photo_text` | `{{text_top}}`, `{{image}}`, `{{text_bottom}}` |
| `photo_top_text_bottom` | `{{image}}`, `{{text_bottom}}` |
| `cta_final` | `{{image}}`, `{{cta_headline}}`, `{{cta_subtext}}`, `{{whatsapp}}` |

## Limites de caracteres recomendados

Pra texto não vazar do canvas em 1080×1350:

| Placeholder | Max chars |
|---|---|
| `headline` | 50 |
| `highlight` | 30 |
| `text_top`, `text_bottom` (template 2/4) | 80 |
| `text_top`, `text_bottom` (template 3) | 60 |
| `cta_headline` | 40 |
| `cta_subtext` | 80 |
| `whatsapp` | 20 (formato: "(11) 99999-9999") |

Esses limites já são considerados no prompt da etapa "Gerar conceito" do n8n.

## Como testar localmente

```bash
# Servir os HTMLs com qualquer http server
npx http-server -p 8080

# Abrir no navegador (substituir placeholders manualmente pra testar visual)
open http://localhost:8080/cover_cinematic.html
```

## Como editar via Claude Code

```bash
git clone https://github.com/<user>/inspira-templates.git
cd inspira-templates
claude code  # ou abre no Cursor com o agente
```

Pede: "ajuste o template `text_photo_text.html` pra que o texto bottom fique mais próximo da foto" e o Claude edita direto.

## Versionamento

Cada commit é uma versão dos templates. Se quiser fazer A/B test, basta criar branch `experimental` e apontar `TEMPLATES_BASE_URL` do Railway pra ele temporariamente.

## Cache

O microsserviço `inspira-render` faz cache dos templates em memória por 5 minutos. Após editar e fazer push, espere até 5min ou redeploy o Railway pra ver as mudanças.
