---
type: Estado
title: Estado do projeto
description: Onde o projeto está agora — a frente corrente, o próximo passo, o que está bloqueado e o que espera decisão.
dono: process:memoria
---

# Estado — 2026-11-23

> Importado pelo `CLAUDE.md`: chega a toda sessão e subagente. Teto de linhas e bytes no perfil.
> Detalhe vai para o conceito; histórico, para o [log](/log.md).

## Frente corrente

Memória montada ([decisão](/decisoes/2026-11-23-memoria-lastro-perfil-interfone.md)). Nenhum código
nem desenho ainda.

Requisitos do usuário (2026-11-23):
- duas unidades: **externa** (câmera, alto-falante, microfone, botão; carcaça HDL) e **interna**
  (telefone HDL da cozinha, placa trocada, mantendo microfone e fone);
- comunicação **sem fio, Wi-Fi**;
- **app de PC e celular**: conversar com a unidade externa e ver a câmera;
- vídeo disponível ao **NVR da casa** (Xiongmai, ver `~/projetos/nvrcap`);
- opcional: **campainhas remotas** para avisar o toque.

Hardware disponível: Orange Pi Zero 2 (1 GB), câmera USB, MAX9814 (microfone com AGC), buzzer
piezo AT3040, cápsula de eletreto WA55A103, PAM8403 (amplificador), ESP32-WROOM, telefone HDL e
peças da caixa externa HDL.

## Próximo passo

Definir a arquitetura: o que roda em qual placa, protocolo de áudio/vídeo e de toque, e como o NVR
recebe a câmera.

## Bloqueado

Nada.

## Espera decisão

Arquitetura (acima).
