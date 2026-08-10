# Driver de LED CRAO — PCB de Conversor Ressonante Auto-Oscilante Multifiltro

> Projeto e layout de uma **placa de circuito impresso (PCB)** para um **driver de iluminação de estado sólido (LEDs)** baseado em um **Conversor Ressonante Auto-Oscilante (CRAO)**, com seleção modular da topologia do filtro ressonante (**LLC, LCC e LC**). O acionamento do meio-ponte é feito por **circuito puramente discreto e analógico**, dispensando circuitos integrados dedicados de *gate drive*.


![EDA](https://img.shields.io/badge/EDA-Altium%20Designer-blue)
![Layers](https://img.shields.io/badge/PCB-2%20camadas-green)
![Status](https://img.shields.io/badge/vers%C3%A3o-1.0-lightgrey)


---

## Sumário

- [Sobre o projeto](#sobre-o-projeto)
- [Especificações principais](#especificações-principais)
- [Características de projeto](#características-de-projeto)
- [Escopo](#escopo)
- [Manufaturabilidade](#manufaturabilidade)
- [Estrutura do repositório](#estrutura-do-repositório)
- [Como abrir e utilizar os arquivos](#como-abrir-e-utilizar-os-arquivos)
- [Autor](#autor)
- [Licença](#licença)

---

## Sobre o projeto

Este repositório contém o **projeto completo de PCB** de um driver modular para LEDs baseado no **Conversor Ressonante Auto-Oscilante (CRAO)**. O layout foi concebido para uma topologia **meio-ponte (half-bridge)**, com **caminhos de corrente de alta frequência reduzidos** para minimizar interações eletromagnéticas.

A principal característica do projeto é a **eliminação de circuitos integrados dedicados ao acionamento das chaves**: a comutação do meio-ponte é realizada por um **circuito discreto e analógico**, baseado em transformador de corrente/tensão inserido no filtro ressonante. A placa permite ainda a **seleção física da topologia do filtro** (LC, LLC ou LCC) por meio de jumpers/chaves.

O trabalho foi desenvolvido como **Projeto Individual** na Universidade Federal de Santa Maria (UFSM), no contexto de um projeto de pesquisa voltado ao desenvolvimento de drivers para iluminação de estado sólido de potências ≥ 50 W, com metodologias aplicáveis de centenas de kHz até a faixa de MHz.

---

## Especificações principais

| Parâmetro | Especificação |
|---|---|
| Topologia de potência | Meio-ponte (half-bridge) |
| Filtros ressonantes suportados | LLC, LCC e LC (seleção modular) |
| Potência na carga (LED) | até 50 W |
| Barramento de entrada | CC universal retificado, 120–400 V |
| Frequência de operação | ajustável de 300 kHz a 1,5 MHz |
| Acionamento das chaves | discreto/analógico (sem CI dedicado) — partida por rede RC + DIAC |
| Comutação | ZVS (operação acima da frequência de ressonância) |
| Monitoramento de corrente | isolado galvanicamente por efeito Hall (ACS712) + pontos de teste |
| Camadas da PCB | 2 (FR-4, 1,6 mm, cobre 1 oz) |

---

## Características de projeto

- **Acionamento auto-oscilante:** partida por rede RC associada a um componente de disparo (DIAC); em regime, os enrolamentos secundários do transformador de comando aplicam sinais complementares aos gates dos MOSFETs, com transição em **ZVS**.
- **Modularidade de filtro:** seleção física de LC / LLC / LCC por jumpers/chaves.
- **Encapsulamentos:** MOSFETs do meio-ponte em DPAK (TO-252); sensor ACS712 em SOIC-8; magnéticos com espaço para indutor série até EFD25/13/9, transformador principal até EE35/18/12 e transformador de corrente com até 2 toroides ∅12,5 mm; passivos majoritariamente 0805.
- **Interfaces:** terminais Faston (entrada/saída), bornes a parafuso (alimentação auxiliar e sinais), entrada de *dimming* 0–10 V, ajustes por trimpots multivoltas e jumpers de medição de corrente que abrem laço para ponteira.
- **Robustez em alta tensão:** *clearance/creepage* ampliados nas malhas de alta tensão, com previsão de recortes (slots) para reforço de isolamento.
- **EMI/ESD:** trilhas de gate emparelhadas e curtas, desacoplamento X7R junto aos estágios de comando, capacitores de tanque com dielétrico de alta estabilidade (NP0/C0G nos críticos, filme FKP nos de potência) e afastamento dos magnéticos das fontes de calor.

---

## Escopo

**Incluído:** captura completa do esquemático e associação de footprints validados; dimensionamento de trilhas e dimensionamento térmico; roteamento de trilhas de alta frequência com minimização de caminho; seleção modular das topologias de filtro; monitoramento de corrente por efeito Hall (ACS712) e pontos de teste; geração dos arquivos industriais (Gerber e NC Drill); montagem e fabricação comercial da placa.

**Fora do escopo (nesta versão):**controle digital em microcontrolador; estágio integrado de Correção de Fator de Potência (CFP).

---

## Manufaturabilidade

Projetado dentro dos limites de um processo padrão de **PCB de 2 camadas**, mantendo custo e reprodutibilidade adequados ao uso em pesquisa:

- Substrato/acabamento: FR-4 de 1,6 mm, cobre 1 oz (35 µm), máscara de solda e serigrafia padrão, acabamento HASL Lead-Free.
- Regras de projeto: largura mínima de trilha 20 mil (sinais/gate) e 200 mil para potência; furo máximo 150 mil (acomoda fixação M3 de ∅3,2 mm); *sliver* de máscara ≥ 5 mil.
- Gestão térmica: *thermal stitching* (vias térmicas) sob as abas de dissipação dos componentes de potência, usando o polígono de potência como dissipador passivo.
- Verificação: validação por **DRC** com inspeção específica das regras de alta tensão (clearance/creepage) e revisão da serigrafia.
- Saídas de fabricação: Gerber (RS-274X) e NC Drill (Excellon), além da BOM.

---

## Estrutura do repositório

> Sugestão de organização — ajuste conforme os arquivos enviados.

```
.
├── projeto-altium/          # Projeto fonte: .PrjPcb, .SchDoc, .PcbDoc
├── esquematico/             # Esquemático completo em PDF
├── fabricacao/
│   ├── gerber/              # Camadas Gerber (RS-274X): cobre, máscara, serigrafia, contorno
│   └── nc_drill/            # Arquivos de furação (Excellon) e mapa de furos
├── bom/                     # Lista de materiais (.xlsx)
└── README.md
```

---

## Como abrir e utilizar os arquivos

- **Projeto fonte (`projeto-altium/`):** requer **Altium Designer** para abrir o `.PrjPcb`, editar o esquemático (`.SchDoc`) e o layout (`.PcbDoc`).
- **Esquemático (`esquematico/`):** o PDF pode ser aberto em qualquer leitor, sem software específico.
- **Fabricação (`fabricacao/`):** os arquivos **Gerber** e **NC Drill** podem ser enviados diretamente a um fabricante de PCB ou inspecionados em um visualizador de Gerber (ex.: gerbv, ou visualizadores online).
- **BOM (`bom/`):** planilha com a lista de materiais para cotação/compra.

> Esta versão corresponde a **peça única** (sem panelização). Os furos M3 (H1–H4) permitem a montagem mecânica da placa e do transformador toroidal.

---

## Autor

**João Vitor Reser da Silva** — Engenharia Elétrica, Universidade Federal de Santa Maria (UFSM).
Relatório: versão 1.0, junho de 2026.

---

## Licença

> **MIT** (permissiva, para os arquivos de projeto)
