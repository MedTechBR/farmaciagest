# FarmáciaGest

Gestão de escala, trocas e banco de horas da farmácia. App de página única (HTML/CSS/JS puro), sem servidor e sem cadastro.

**No ar:** https://medtechbr.github.io/farmaciagest/

## O que faz

- **Funcionários** — cadastro por função (balconista, entregador, caixa), com importação da escala a partir de planilha `.xlsx`/`.csv` (reconhecimento automático de colunas + pré-visualização antes de confirmar).
- **Escala** — em dois modos: **Dia** (quem trabalha em cada turno, folgas, trocas, matriz de cobertura mínima e resumo do dia) e **Semana** (os 7 dias lado a lado, com marcação de desfalque e feriado; clicar num dia abre a escala completa dele). Botão **Imprimir** com folha de estilo própria para colar no balcão.
- **Trocas** — registro de trocas entre funcionários. No dia da troca aparece um **`*`** ao lado do nome na escala; tocar nele abre o detalhe (escala original, o que a pessoa passou a fazer, com quem trocou, dias e observação).
- **Banco de horas** — **Adicionar** (hora extra) ou **Descontar** (compensação, falta, saída antecipada) com saldo por funcionário. As horas são digitadas em positivo (aceita `1,5` ou `1.5`) e o sinal vem do botão.
- **Recebimento** — entrada de mercadoria: anexa documento, lista os produtos com a distribuidora entre parênteses, busca, filtros e caixa para marcar o que chegou (ver abaixo).
- **Divergências** — pendências de nota fiscal: nota, produto, distribuidora, quantidade, motivo da devolução, NF de devolução e situação. Dá para abrir já preenchido a partir de um item do recebimento.
- **Escala mensal** — importa a planilha de escala que a farmácia já usa (grade funcionário × dia) e gera a contagem de turnos por pessoa (ver abaixo).
- **Configurações** — mínimos por turno/categoria, turnos, categorias, feriados, backup (exportar/importar) e **banco de dados de teste**.

## Leitura de documentos no recebimento

Tudo é processado no próprio navegador — nenhum arquivo sai do aparelho. O documento **não fica salvo** (localStorage não comporta PDF nem foto); só os itens extraídos.

| Formato | Como é lido | Confiabilidade |
|---|---|---|
| **XML da NF-e** | `DOMParser` nos campos `xProd`/`qCom`/`emit` | Exata — é o formato recomendado |
| **PDF** | camada de texto via pdf.js (carregado sob demanda) | Boa em PDF digital; PDF digitalizado não tem texto e é recusado com aviso |
| **Imagem/foto** | OCR via tesseract.js em português (carregado sob demanda) | Aproximada — erra caracteres (num teste, `C/36` virou `C/86`) |
| **TXT/CSV/texto colado** | heurística de linha | Boa em lista e tabela de pedido |

Por isso **toda** extração passa por uma tela de conferência editável antes de entrar na lista: dá para desmarcar linhas, corrigir nome e ajustar quantidade. A heurística descarta cabeçalho, rodapé, totais e razão social, e entende quantidade nas duas ordens (`CX 20` e `20 CX`).

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

## Escala mensal (importar a planilha existente)

A aba lê a planilha no formato que a farmácia já usa — uma linha por funcionário, os dias do mês nas colunas — sem exigir modelo novo. Códigos reconhecidos:

| Código | Significado |
|---|---|
| `H01`–`H09` | turno trabalhado (H01 06:50–14:40, H02 08:10–16:00, H03 10:10–18:00, H04 12:10–20:00, H05 13:10–21:00, H06 14:30–22:20, H07 15:30–23:20, H08 09:00–13:00 / 15:00–19:00, H09 09:30–19:20) |
| `F` | folga |
| `*` | domingo escalado para trabalhar |
| células que soletram `F-É-R-I-A-S` | férias |
| `FF` / `FA` | folga de feriado / de aniversário |

**Por que não depende da cor:** o SheetJS (leitor de planilha no navegador) não expõe o preenchimento das células. A classificação é feita pelo conteúdo, e foi conferida contra as cores reais do arquivo (amarelo = folga/domingo, verde = férias): **as duas classificações coincidem em 100% das células dos 21 funcionários de agosto/2026**. O bloco de férias é distinguido de uma folga isolada por ser uma sequência de 3+ letras de `FÉRIAS`.

**A escala mensal manda no dia a dia.** O horário de cada pessoa muda conforme o dia, então, para as datas de um mês importado, a aba Escala usa o turno da grade (`H0x` com o horário oficial; `F`/férias/`FF`/`FA` como folga; `*` como domingo escalado). O turno do cadastro do funcionário é apenas o *habitual*, usado como reserva em datas sem escala importada. A importação também cria os turnos que só existem na planilha (ex.: H04, H09), senão quem fosse escalado neles não apareceria na Escala do dia.

Saídas: grade colorida do mês, **contagem por funcionário** (quantos dias em cada turno, folgas, domingos, férias, dias trabalhados), export CSV e o botão **Cadastrar funcionários**, que cria as pessoas na aba Funcionários com a função da planilha e o turno em que cada uma mais trabalhou. Folga fixa só é atribuída quando cai no mesmo dia da semana 3+ vezes — nessa escala a folga é rodiziada.
