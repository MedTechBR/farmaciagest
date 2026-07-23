# FarmáciaGest

Gestão de escala, trocas e banco de horas da farmácia. App de página única (HTML/CSS/JS puro), sem servidor e sem cadastro.

**No ar:** https://medtechbr.github.io/farmaciagest/

## O que faz

- **Funcionários** — cadastro por função (balconista, entregador, caixa), com importação da escala a partir de planilha `.xlsx`/`.csv` (reconhecimento automático de colunas + pré-visualização antes de confirmar).
- **Escala diária** — quem trabalha em cada dia, folgas e extras.
- **Trocas** — registro de trocas de plantão entre funcionários.
- **Banco de horas** — saldo positivo/negativo por funcionário.
- **Configurações** — mínimos por turno/categoria, turnos, categorias, feriados, backup (exportar/importar) e **banco de dados de teste**.

## Banco de dados de teste

Em **Configurações → Banco de dados de teste**, o botão *Criar banco de teste* povoa o app com uma farmácia fictícia: 12 funcionários nos 3 turnos (com folgas semanais, hora extra e folga de feriado), 3 trocas, 8 lançamentos de banco de horas e os 4 próximos feriados nacionais.

Todo registro criado assim nasce marcado com `demo:true`, aparece com o selo **teste** na lista de funcionários e some junto no botão *Remover dados de teste*. Dados reais nunca são tocados — dá para criar o banco de teste por cima de um cadastro de verdade e depois limpar só o que é fictício.

## Dados

Tudo fica salvo **apenas no navegador** (`localStorage`). Nada é enviado para servidor algum. Trocar de navegador ou limpar os dados do site apaga tudo — use **Configurações → Backup** para exportar.

## Rodar localmente

Basta abrir o `index.html` no navegador. Não há build nem dependências para instalar.

## Stack

HTML/CSS/JS vanilla em arquivo único. Via CDN: [Inter](https://fonts.google.com/specimen/Inter), [Tabler Icons](https://tabler.io/icons) e [SheetJS](https://sheetjs.com/) (leitura/escrita de planilhas).
