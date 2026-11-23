---
type: Integracao
title: Orange Pi Zero 2 (H616) — o que a placa faz e o que não faz
description: Limites da Orange Pi Zero 2 que moldam a unidade externa, segundo o manual oficial — sem entrada de microfone (nem pela placa de expansão), sem codec de vídeo por hardware no Linux, Wi-Fi 802.11ac, 3 USB 2.0.
dono: process:memoria
tags: [orange-pi, h616, unidade-externa, audio, video]
status: deprecated
mem_substituido_por: /integracoes/raspberry-pi-4.md
generated:
  by: claude-code/claude-opus-5-5
  at: 2026-11-23T14:00:00-03:00
sources:
  - id: manual
    resource: "~/Downloads/OrangePi_Zero2_H616_User manual_v3.7.pdf — 1.4 Hardware Features (p. 1), 13pin expansion board (p. 8), 3.2 Linux core driver matching (p. 47), 3.13.3 USB Microphone Test (p. 115), 3.17 13 Pin expansion board (p. 136)"
  - id: imagem
    resource: "Imagem Orangepizero3_1.0.6_ubuntu_noble_desktop_xfce_linux6.1.31.img (~/Downloads), lida com debugfs em 2026-11-23: /boot/config-6.1.31-sun50iw9, System.map, /lib/modules, dtb sun50i-h616-orangepi-zero2.dtb, /usr/lib/aarch64-linux-gnu"
stale_after: 2027-05-23T00:00:00-03:00
---

# Orange Pi Zero 2 (H616)

| item | o que o manual diz |
|---|---|
| CPU / RAM | 4× Cortex-A53 (H616), 1 GB DDR3 compartilhada com a GPU |
| Wi-Fi / BT | AW859A, 802.11 a/b/g/n/ac (2,4 e 5 GHz), BT 5.0 |
| Ethernet | 10/100/1000 |
| USB | 3× USB 2.0 host: 1 na placa e 2 pelo conector de 13 pinos (placa de expansão) |
| áudio | **só saída**: LINE OUT (fone) pela placa de expansão, e HDMI |
| microfone | **"No support, no support, no support!"** — o Mic da placa de expansão não funciona na Zero 2; o manual manda usar microfone USB |
| codec de vídeo | **"Video codec: NO"** no Linux 4.9 e no 5.16 — codificação H.264 só em software |
| 26 pinos | I2C3, SPI1, UART5, PWM e GPIO |
| 13 pinos | com a placa de expansão, os pinos 10, 11 e 12 ficam ocupados |
| kernels oficiais | Linux 4.9.170 e 5.16.17 (Debian 12, Ubuntu 22.04) |

## Kernel BSP 6.1.31 (conferido na imagem)

O kernel `6.1.31-sun50iw9` da Orange Pi é o mesmo para Zero 2, Zero 2W e Zero 3: a imagem da Zero 3
traz o `sun50i-h616-orangepi-zero2.dtb`.

- O DTB da Zero 2 declara o motor de vídeo: `video-codec@1c0e000`,
  `compatible = "allwinner,sun50i-h616-video-engine"`.
- O único driver para ele é o **Cedrus do kernel principal** (`CONFIG_VIDEO_SUNXI_CEDRUS=m`,
  `sunxi-cedrus.ko`), que **só decodifica**. Não há driver de fábrica `cedar_ve` (`/dev/cedar_dev`)
  na config, nos símbolos nem nos módulos, e não há `libcedarc`/`libvencoder` no sistema.
- Para codificar em software já vêm `libx264.so.164`, `libavcodec.so.60` (FFmpeg 6.1) e GStreamer.
- Áudio USB: `CONFIG_SND_USB_AUDIO=m`. O codec interno é `CONFIG_SND_SUN50IW9_CODEC` (só saída).

## O que isso implica no interfone

- **Microfone da porta via USB.** Uma placa de som USB com entrada de microfone alimenta o eletreto
  WA55A103 direto. Usar a mesma placa para a saída (→ PAM8403) deixa captura e reprodução no mesmo
  relógio, o que um cancelador de eco precisa.
- **USB conta:** câmera + placa de som = 2 das 3 portas, por isso a placa de expansão continua útil.
- **H.264 para o NVR é software** nos 4 núcleos: confira resolução, fps e temperatura antes de fechar.
- **Wi-Fi dual band**, mas o ESP32 do telefone só fala 2,4 GHz: os dois precisam estar na mesma rede local.

## Como reconferir

O manual na pasta Downloads (páginas acima). Na placa: `arecord -l` (só deve aparecer o cartão USB) e
`ls /dev/video*`.
