---
type: Decisao
title: Memória do projeto no lastro, perfil de código adaptado a embarcado e desenho
description: O interfone usa o lastro (OKF v0.2) com o perfil de código, mais o tipo Peca para desenhos, o escopo bancada para gravação de firmware, captura de gravação de placa e do FreeCAD MCP, e commit+push automático da memória.
dono: human:vitware
tags: [memoria, lastro, perfil]
status: draft
generated:
  by: claude-code/claude-opus-5-5
  at: 2026-11-23T12:50:00-03:00
sources:
  - id: pedido
    resource: "Usuário em 2026-11-23: configurar o lastro antes de qualquer trabalho; o projeto terá programação, desenhos de componentes e outros"
  - id: lastro
    resource: "~/lastro (vpfeltrim/lastro, commit d427df4+1), templates/memoria-codigo.yaml"
  - id: referencias
    resource: "Perfis já em uso: ~/projetos/nvrcap/.claude/memoria.yaml (código, Orange Pi, NVR) e ~/projetos/model-3d-home/.claude/memoria.yaml (tipo Peca, captura do FreeCAD MCP)"
---

# Memória no lastro, perfil adaptado ao interfone

## Decisão

Adotar o lastro com `lastro init --perfil codigo` e adaptar o perfil (`.claude/memoria.yaml`):

- **Tipo `Peca`** (`pecas/`), exigindo `sources` e `mem_codigo`: desenho físico (esquema de ligação,
  pinagem, placa, chicote, suporte impresso, adaptação da carcaça HDL), ancorado nos arquivos de
  `hardware/` e `mecanica/`. O lint avisa `A-DERIVA` quando o desenho anda sem a ficha.
- **Escopo `bancada`** na `Atividade`: gravar firmware ou configurar placa de teste. Sai do git, mas
  se desfaz regravando, então não exige `Decisao`. `mudanca` (exige autorização) fica para o que
  está instalado na casa: unidades montadas, NVR, rede, roteador.
- **Captura** de gravação de firmware (esptool, espefuse, idf.py, pio upload, arduino-cli, mpremote),
  imagem no SD, configuração de rede do Orange Pi e ferramentas do FreeCAD MCP que alteram modelo.
  Modo estrito em `firmware/`, `software/`, `scripts/`, `hardware/`, `mecanica/`; testes, saída de
  build e backups do FreeCAD/KiCad ignorados.
- **Rótulos de log** a mais: `Desenho`, `Gravação`, `Montagem`.
- **Segredos**: um único `.env` na raiz, fora do git. Firmware e app geram o header de credencial a
  partir dele numa pasta ignorada.
- **Git**: commit e push automáticos da memória no fim do turno (`git.caminhos`), como nos demais
  projetos.

## Por quê

O projeto mistura três naturezas de artefato (firmware embarcado, software de PC/celular e desenho
físico) e três lugares onde as mudanças acontecem (repositório, bancada, casa). O perfil de código
cobre a primeira e a segunda. O tipo `Peca` cobre o desenho, e o escopo `bancada` evita exigir uma
decisão a cada gravação de firmware sem afrouxar o que mexe na casa.

## Armadilha registrada

`segredos.arquivos` usa `glob` sem recursão, e o trecho literal de cada padrão vira agulha de
bloqueio. Um padrão como `.../interfone/**/secrets*.h` marcaria qualquer caminho do projeto como
segredo. Também não interpreta `#define` em C. Por isso há um `.env` só, em `KEY=VALOR`.
