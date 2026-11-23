---
type: Integracao
title: Raspberry Pi 4 (4 GB) — servidor único do interfone, na porta
description: O que o Raspberry Pi 4 oferece à unidade externa — codificador H.264 por hardware (1080p30) pelo V4L2, 4 USB, Wi-Fi 802.11ac, 40 pinos com I2S — e o que não oferece (entrada de microfone), além do tamanho maior que o da Orange Pi Zero 2.
dono: process:memoria
tags: [raspberry-pi, unidade-externa, video, h264, audio]
status: draft
generated:
  by: claude-code/claude-opus-5-5
  at: 2026-11-23T16:00:00-03:00
sources:
  - id: especificacao
    resource: "https://www.raspberrypi.com/products/raspberry-pi-4-model-b/specifications/ (BCM2711 4×A72 1,8 GHz; H.264 1080p60 decode, 1080p30 encode; 2×USB 3.0 + 2×USB 2.0; 802.11ac; 40 pinos; 5 V 3 A USB-C)"
  - id: v4l2
    resource: "Driver bcm2835-codec do kernel Raspberry Pi: codificador stateful V4L2 (/dev/video11), usado pelo FFmpeg como h264_v4l2m2m — conhecimento prévio, a conferir na placa com `v4l2-ctl --list-devices`"
  - id: usuario
    resource: "Usuário em 2026-11-23: trocar a Orange Pi Zero 2 por um Raspberry Pi 4 de 4 GB; tudo da aplicação roda nele, sem serviço em outros computadores"
stale_after: 2027-05-23T00:00:00-03:00
---

# Raspberry Pi 4 (4 GB)

| item | valor |
|---|---|
| CPU / RAM | BCM2711, 4× Cortex-A72 a 1,8 GHz; 4 GB |
| vídeo | **H.264 por hardware: codifica 1080p30**, decodifica 1080p60 (o Pi 5 não tem mais codificador; o Pi 4 tem) |
| USB | 2× USB 3.0 + 2× USB 2.0 — câmera e Ugreen sem placa extra |
| rede | Wi-Fi 802.11ac (2,4 e 5 GHz), BT 5.0, Ethernet gigabit |
| áudio | P2 de 4 polos **só de saída** (PWM): continua sem microfone, a Ugreen US205 segue necessária |
| 40 pinos | GPIO, I2C, SPI, UART, PWM e **I2S** (GPIO 18–21) |
| tamanho | 85 × 56 mm (a Zero 2 tinha 53 × 60 mm) |
| alimentação | 5 V 3 A por USB-C |

## O que muda no interfone

- **Vídeo:** captura (YUYV ou MJPEG) → `h264_v4l2m2m` → RTSP. A CPU fica livre para o áudio, o
  cancelamento de eco e o servidor. Também é possível usar libx264 se precisar (os A72 são ~3× mais
  rápidos que os A53 do H616).
- **Ponto de atenção do codificador:** a altura de 1080 vira 1088 internamente; prefira 720p ou
  confira o formato depois de configurar.
- **Mecânica:** a placa é maior. É preciso confirmar se cabe na carcaça HDL da porta.

## Como reconferir

Na placa: `v4l2-ctl --list-devices` (deve listar `bcm2835-codec` com `/dev/video11` de codificação),
`ffmpeg -hide_banner -encoders | grep v4l2m2m`, `arecord -l` (só a Ugreen).
