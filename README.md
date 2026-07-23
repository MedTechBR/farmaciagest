# FarmáciaGest

Gestão de escala, trocas e banco de horas da farmácia. App de página única (HTML/CSS/JS puro), sem servidor e sem cadastro.

**No ar:** https://medtechbr.github.io/farmaciagest/

## O que faz

- **Funcionários** — cadastro por função (balconista, entregador, caixa), com importação da escala a partir de planilha `.xlsx`/`.csv` (reconhecimento automático de colunas + pré-visualização antes de confirmar).
- **Escala diária** — quem trabalha em cada dia, folgas e extras.
- **Trocas** — registro de trocas de plantão entre funcionários.
- **Banco de horas** — saldo positivo/negativo por funcionário.
- **Configurações** — backup (exportar/importar) dos dados.

## Dados

Tudo fica salvo **apenas no navegador** (`localStorage`). Nada é enviado para servidor algum. Trocar de navegador ou limpar os dados do site apaga tudo — use **Configurações → Backup** para exportar.

## Rodar localmente

Basta abrir o `index.html` no navegador. Não há build nem dependências para instalar.

## Stack

HTML/CSS/JS vanilla em arquivo único. Via CDN: [Inter](https://fonts.google.com/specimen/Inter), [Tabler Icons](https://tabler.io/icons) e [SheetJS](https://sheetjs.com/) (leitura/escrita de planilhas).
