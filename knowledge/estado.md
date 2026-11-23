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

Arquitetura inicial decidida ([decisão](/decisoes/2026-11-23-arquitetura-inicial.md)): Orange Pi
Zero 2 na porta (câmera USB, placa de som USB com eletreto WA55A103 → PAM8403, botão), ESP32 no
telefone HDL (MAX9814, fone, gancho, toque), campainhas com D1 mini / S2 mini. Servidor próprio em
Go: RTSP para o NVR, áudio e toque. Câmera só MJPEG/YUY2 ([ficha](/integracoes/camera-usb-2mp.md)); a placa não tem microfone nem codec
de vídeo ([ficha](/integracoes/orange-pi-zero-2.md)).

Requisitos (usuário, 2026-11-23): Wi-Fi; app de PC e celular para conversar e ver a câmera; vídeo no
NVR Xiongmai (ver `~/projetos/nvrcap`); campainhas remotas opcionais. Energia: por conta do usuário.

## Próximo passo

Testes de bancada que fecham as dúvidas da decisão:
1. placa de som USB para a porta: escolher/comprar e testar com o WA55A103 (`arecord -l`);
2. formatos reais da câmera (`v4l2-ctl --list-formats-ext`) e custo do H.264 em software;
3. protocolos que o NVR aceita ao adicionar canal (RTSP ou só ONVIF);
4. impedância do fone do monofone (multímetro).

## Bloqueado

Nada.

## Espera decisão

Protocolo de áudio e controle entre porta, telefone e app; como o app recebe vídeo e áudio.
