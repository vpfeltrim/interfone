---
type: Decisao
title: Arquitetura inicial — Orange Pi na porta, ESP32 no telefone, servidor Go leve
description: Divisão de hardware entre a unidade externa (Orange Pi Zero 2), o telefone (ESP32) e as campainhas (D1 mini / S2 mini), servidor RTSP próprio em Go em vez de go2rtc/MediaMTX, e o que ainda falta confirmar.
dono: human:vitware
tags: [arquitetura, hardware, audio, video]
status: draft
generated:
  by: claude-code/claude-opus-5-5
  at: 2026-11-23T13:30:00-03:00
sources:
  - id: usuario
    resource: "Usuário em 2026-11-23, respostas aos 7 pontos da proposta de arquitetura (placa de expansão com mic e saída de áudio, WA55A103 na porta, câmera MJPEG/YUY2, RTSP com Go próprio, D1 mini/S2 mini nas campainhas, MAX9814 no telefone, energia resolvida por ele)"
  - id: foto-monofone
    resource: "Foto do monofone HDL aberto, 2026-11-23: cápsula receptora (fio vermelho/azul) no topo, eletreto pequeno embaixo, cordão preto"
  - id: camera
    resource: /integracoes/camera-usb-2mp.md
  - id: placa
    resource: /integracoes/orange-pi-zero-2.md
  - id: usuario-2
    resource: "Usuário em 2026-11-23: tem interface de áudio USB (foto: Ugreen US205, P/N 30724, USB 2.0 External Stereo Sound Adapter, 5 V 0,1 A) e o NVR aceita RTSP"
---

# Arquitetura inicial

## Decidido (usuário, 2026-11-23)

| unidade | placa | periféricos |
|---|---|---|
| **Porta** (carcaça HDL) | Orange Pi Zero 2 + placa de expansão de 13 pinos (USB extras) | câmera USB; **placa de som USB Ugreen US205** com o eletreto WA55A103 na entrada de microfone e a saída → PAM8403 → alto-falante; botão |
| **Telefone** (HDL, cozinha) | ESP32-WROOM | MAX9814 (microfone); fone do monofone; PCM5102 se o fone precisar; gancho; toque |
| **Campainhas remotas** | Wemos D1 mini (ESP8266) / S2 mini (ESP32-S2) | buzzer (AT3040 disponível) |

- O Orange Pi é o centro do sistema. Ele recebe o botão, toca o telefone e as campainhas, entrega o
  vídeo ao NVR por **RTSP** e atende o app.
- **Software próprio em Go, leve**, em vez de go2rtc ou MediaMTX: só o que o interfone usa.
- A energia de cada unidade é responsabilidade do usuário. Não é premissa de projeto.

## Por quê

A câmera é USB, e o ESP32-WROOM não tem USB host, então só o Orange Pi lê a câmera. O telefone
precisa de pouco processamento e de baixo consumo, o que cabe no ESP32. As placas menores já estão
disponíveis para as campainhas.

## Em aberto (confirmar antes de fechar o desenho)

1. ~~Entrada de microfone do H616~~ — **resolvido pelo manual**: a Zero 2 não tem microfone, nem
   pela placa de expansão ([ficha](/integracoes/orange-pi-zero-2.md)). O microfone da porta vai pela
   placa de som USB do usuário (Ugreen US205). Medir a tensão de alimentação no conector de microfone
   antes de ligar o WA55A103.
2. **Eco na porta.** Trocar a cápsula melhora a captação, mas não impede o alto-falante de entrar no
   microfone. Plano: isolar o microfone mecanicamente e, se não bastar, cancelar o eco no Go (speexdsp
   via cgo) ou baixar o microfone enquanto o telefone fala.
3. **H.264 para o NVR.** A câmera só entrega MJPEG/YUY2 e a placa não tem codec de vídeo no Linux
   (manual: "Video codec: NO"), então o Orange Pi codifica em software.
   Validar resolução, fps e temperatura dentro da caixa.
4. ~~Protocolo de canal do NVR~~ — **resolvido pelo usuário**: o NVR aceita RTSP. Não precisa de ONVIF.
5. **Impedância do fone do monofone.** Medir com o multímetro para decidir entre o DAC do ESP32 com
   capacitor de acoplamento e o PCM5102.

## Como desfazer

Nova `Decisao` substituindo esta (`status: deprecated` + `mem_substituido_por`).
