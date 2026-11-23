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
Zero 2 na porta (câmera USB, placa de som USB Ugreen US205 com eletreto WA55A103 → PAM8403, botão), ESP32 no
telefone HDL (MAX9814, fone, gancho, toque), campainhas com D1 mini / S2 mini. Servidor próprio em
Go: RTSP para o NVR, áudio e toque. Câmera só MJPEG/YUY2 ([ficha](/integracoes/camera-usb-2mp.md)); a placa não tem microfone nem codec
de vídeo ([ficha](/integracoes/orange-pi-zero-2.md)).

Requisitos (usuário, 2026-11-23): Wi-Fi; app de PC e celular para conversar e ver a câmera; vídeo no
NVR Xiongmai (ver `~/projetos/nvrcap`); campainhas remotas opcionais. Energia: por conta do usuário.

## Próximo passo

Testes de bancada que fecham as dúvidas da decisão:
1. Ugreen US205 + WA55A103 na Orange Pi (`arecord -l`, gravação curta; medir a tensão no microfone);
2. formatos reais da câmera (`v4l2-ctl --list-formats-ext`) e custo do H.264 em software (x264);
3. impedância do fone do monofone (multímetro).

Vídeo: libx264 por software, com o encoder parametrizável; hardware só como experimento com kernel
próprio ([análise](/analises/codificacao-h264-no-h616.md)). O NVR aceita RTSP (usuário).

**Hardware fechado** (2026-11-23). Sem fechadura; iluminação noturna adiada; campainhas na fase final.

## Bloqueado

Nada.

## Espera decisão

Próxima frente: esquemas de ligação (`hardware/`) ou protocolo de áudio/controle entre porta,
telefone e app (como o app recebe vídeo e áudio).
