---
type: Integracao
title: Câmera USB 2 MP (MJPEG/YUY2, grande angular)
description: Módulo de câmera UVC da unidade externa — 1920×1080, só MJPEG e YUY2 (sem H.264), 130°/150°, foco fixo de 20 a 100 cm, imagem estável entre 0 e 50 °C.
dono: process:memoria
tags: [camera, uvc, video, unidade-externa]
status: draft
generated:
  by: claude-code/claude-opus-5-5
  at: 2026-11-23T13:30:00-03:00
sources:
  - id: anuncio
    resource: "Ficha do módulo passada pelo usuário em 2026-11-23 (especificação do vendedor, não medida)"
stale_after: 2027-01-23T00:00:00-03:00
---

# Câmera USB 2 MP

| campo | valor (vendedor) |
|---|---|
| sensor | 2 MP, até 1920×1080 |
| formatos | **MJPEG e YUY2** — sem H.264 |
| campo de visão | 130° ou 150° (grande angular, distorção de barril) |
| foco | fixo, objeto a 20–100 cm |
| lente | 4P + IR, f/2,0 |
| temperatura | opera de −20 a 70 °C; imagem estável de 0 a 50 °C |
| alimentação | barramento USB 2.0; cabo de 50 cm; 35 × 15 × 13,1 mm |

## O que isso implica

- **O NVR quer H.264**, então o Orange Pi decodifica o MJPEG e codifica em software (o codificador de
  hardware do H616 não é usável no kernel principal). A resolução e o fps saem de medição; espere 720p
  ou menos.
- **Foco de 20 a 100 cm** combina com o visitante na frente da porta, mas o fundo vai sair desfocado.
- **"+IR"** costuma indicar filtro de infravermelho: sem visão noturna. À noite, precisa de luz.
- **0–50 °C para imagem estável**: a caixa ao sol pode passar disso.

## Como reconferir

No Orange Pi ou no PC: `v4l2-ctl --list-formats-ext -d /dev/videoN` (formatos, resoluções e fps
reais) e `lsusb` (VID:PID). Substitua esta tabela pelo que for medido.
