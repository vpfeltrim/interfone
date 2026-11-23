---
type: Analise
title: Codificação H.264 no H616 — software agora, hardware como experimento
description: Situação em 2026-11 da codificação H.264 por hardware no H616/H618 (nenhum kernel oficial expõe; três ports comunitários de 2026 fora da árvore) e a recomendação de usar libx264 por software com o encoder parametrizável no Go.
dono: process:memoria
tags: [video, h264, h616, cedrus, unidade-externa]
status: draft
generated:
  by: claude-code/claude-opus-5-5
  at: 2026-11-23T15:00:00-03:00
sources:
  - id: imagem
    resource: /integracoes/orange-pi-zero-2.md
  - id: mainline-decode
    resource: "https://github.com/torvalds/linux/commit/1f24d4307515 e https://github.com/torvalds/linux/commit/eabc3c090e81 (H616 no Cedrus, só decodificação, v7.4-rc1)"
  - id: bootlin
    resource: "https://bootlin.com/blog/open-source-linux-kernel-support-for-the-allwinner-v3-v3s-s3-h-264-video-encoder/ (encoder stateless só para V3/V3s/S3, fora do mainline)"
  - id: tvbian
    resource: "https://github.com/sormy/tvbian/tree/main/patches/linux-sunxi (patches 0069–0084 e 0803; testado em H313, kernels 6.18/7.2)"
  - id: gilankpam
    resource: "https://github.com/gilankpam/sbc-groundstations/pull/7 (encoder Cedrus no Orange Pi Zero 2W/H618, kernel 6.18, QP fixo)"
  - id: lubancat
    resource: "https://github.com/Evsdrg/lubancat-a1io-mainline (módulo contra o kernel 6.1.31 da Orange Pi, userspace próprio)"
  - id: bsp-orangepi
    resource: "https://github.com/orangepi-xunlong/linux-orangepi — cedar-ve só nos ramos 4.9/5.4 do sun50iw9; o 6.1 só tem cedrus de decodificação"
  - id: forum-zero2w
    resource: "https://forum.armbian.com/topic/50617-x264-hw-encoding-on-orange-pi-zero-2w/ (2025: 'the driver for h264 encoding does not exist')"
  - id: caso-igual
    resource: "https://github.com/bootlin/v4l2-cedrus-enc-test/issues/1 (câmera USB → RTSP no Zero 2: o autor trocou de placa)"
  - id: pizero2-sw
    resource: "https://forum.core-electronics.com.au/t/pi-zero-w-2-camera-3-cctv/22982 (libx264 ultrafast 1080p ~4 fps num 4×A53 a 1 GHz)"
---

# Codificação H.264 no H616

## Pergunta

A unidade externa precisa entregar H.264 ao NVR por RTSP, e a câmera só fornece MJPEG/YUY2. Dá
para usar o motor de vídeo do H616?

## O que as fontes mostram (pesquisa de 2026-11-23)

- **Kernels oficiais: não.**
  - O BSP 6.1.31 da Orange Pi só tem o Cedrus de decodificação, conferido na
    [imagem](/integracoes/orange-pi-zero-2.md).
  - O driver de fábrica `cedar-ve` (`/dev/cedar_dev`) existia nos ramos 4.9 e 5.4, mas a Orange Pi
    não distribui `libcedarc` para o sun50iw9, e não há relato de codificação funcionando com ele.
  - O mainline ganhou a decodificação do H616 só na v7.4, e a uAPI de encoder stateless ainda não
    entrou.
- **Ports comunitários de 2026, fora da árvore:** os três abaixo funcionam, segundo os próprios autores.

  | port | onde foi testado | alcance |
  |---|---|---|
  | tvbian | TV box H313, kernels 6.18/7.2 | o mais completo: rate control, MJPEG em hardware, patches de FFmpeg para `h264_v4l2m2m` |
  | gilankpam | Orange Pi Zero 2W (H618) | QP fixo, sem rate control |
  | lubancat | kernel 6.1.31 da Orange Pi | userspace próprio, validação fraca |

  Nenhum foi testado no Zero 2.
- **Beco sem saída:** FFmpeg-Cedrus, `cedrus264` e gst-plugin-cedar (feitos para o H3 e para o
  `/dev/cedar_dev` antigo).

## Conclusão

1. **Agora: libx264 por software** no kernel BSP:
   `-c:v libx264 -preset ultrafast -tune zerolatency -profile:v baseline`, 720p a 10–15 fps, GOP de ~2 s.
   Testar entrada YUY2 contra MJPEG. Dissipador obrigatório; medir CPU e temperatura.
2. **No Go, o encoder é parâmetro** da linha do ffmpeg, para trocar por `h264_v4l2m2m` sem mexer no
   resto.
3. **Hardware fica como experimento opcional**: kernel próprio (Armbian ou mainline 6.18+) com os
   patches do tvbian. Riscos:
   - código novo, de um mantenedor só, sem teste no Zero 2;
   - pouca memória CMA com 1 GB;
   - altura alinhada em 1088;
   - Wi-Fi e áudio precisam ser reconferidos no kernel novo.

## Limites

- Não há benchmark de x264 no H616. Os números de 720p são inferência a partir do H3 e do Pi Zero 2 W,
  e viram `Baseline` quando medidos na placa.
- A equivalência entre o H313 e o H616 no encoder é inferência.
