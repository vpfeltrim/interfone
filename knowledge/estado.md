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

**Hardware fechado** ([arquitetura](/decisoes/2026-11-23-arquitetura-inicial.md)). **Raspberry Pi 4
(4 GB)** na porta, no lugar da Orange Pi ([troca](/decisoes/2026-11-23-raspberry-pi-4-na-porta.md)),
como único servidor: câmera USB (só MJPEG/YUY2, [ficha](/integracoes/camera-usb-2mp.md)) → H.264
por hardware → RTSP para o NVR; Ugreen US205 com eletreto WA55A103 → PAM8403 → alto-falante HDL
8 Ω 0,5 W; botão. ESP32 no telefone HDL (MAX9814, fone, gancho, AT3040 no toque). Campainhas
(D1/S2 mini) na fase final. Sem fechadura; iluminação noturna adiada. Software próprio em Go.

## Próximo passo

1. Confirmar se o Pi 4 (85 × 56 mm) cabe na carcaça HDL da porta.
2. Bancada: Ugreen + WA55A103 (`arecord -l`, tensão no microfone); câmera (`v4l2-ctl
   --list-formats-ext`) → `h264_v4l2m2m`; impedância do fone do monofone.
3. Depois: esquemas de ligação (`hardware/`) e protocolo porta ↔ telefone ↔ app.

## Bloqueado

Nada.

## Espera decisão

Acesso fora de casa e notificação no celular sem serviço externo: só na rede local, ou VPN/túnel?
