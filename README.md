# FarmáciaGest

Gestão de escala, trocas e banco de horas da farmácia. App de página única (HTML/CSS/JS puro), sem servidor e sem cadastro.

**No ar:** https://medtechbr.github.io/farmaciagest/

## O que faz

- **Funcionários** — cadastro por função (balconista, entregador, caixa), com importação da escala a partir de planilha `.xlsx`/`.csv` (reconhecimento automático de colunas + pré-visualização antes de confirmar).
- **Escala** — em dois modos: **Dia** (quem trabalha em cada turno, folgas, trocas, matriz de cobertura mínima e resumo do dia) e **Semana** (os 7 dias lado a lado, com marcação de desfalque e feriado; clicar num dia abre a escala completa dele). Botão **Imprimir** com folha de estilo própria para colar no balcão.
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

## Design

Sistema de tokens (cor, espaço, raio, sombra, anel de foco) em CSS variables, com **tema claro e escuro** — segue o sistema por padrão e o botão no cabeçalho fixa a preferência. Todos os pares de texto verificados em ≥ 4,5:1 nos dois temas.

Acessibilidade: abas são um `tablist` real (setas/Home/End), modal com `role="dialog"`, Esc, armadilha de foco e devolução do foco ao elemento que o abriu, `aria-label` em todo botão só-de-ícone, inputs a 16px no mobile (sem zoom automático do iOS), alvos de toque ≥ 44px e suporte a `prefers-reduced-motion`.

## Stack

HTML/CSS/JS vanilla em arquivo único. Via CDN: [Inter](https://fonts.google.com/specimen/Inter), [Tabler Icons](https://tabler.io/icons) e [SheetJS](https://sheetjs.com/) (leitura/escrita de planilhas).
