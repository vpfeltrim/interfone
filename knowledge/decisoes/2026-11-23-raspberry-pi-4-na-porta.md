---
type: Decisao
title: Raspberry Pi 4 no lugar da Orange Pi Zero 2, como servidor único
description: A unidade externa passa a usar um Raspberry Pi 4 de 4 GB, que também é o único servidor do sistema — RTSP para o NVR, áudio, toque e app rodam nele, sem serviço em outro computador.
dono: human:vitware
tags: [arquitetura, hardware, raspberry-pi]
status: draft
generated:
  by: claude-code/claude-opus-5-5
  at: 2026-11-23T16:00:00-03:00
sources:
  - id: usuario
    resource: "Usuário em 2026-11-23: 'vamos substituir a orangepi por uma raspberry pi 4 de 4GB, assim teremos mais controle e tudo que irá gerenciar essa aplicação irá rodar nele, sem nenhum outro serviço em outros computadores'"
  - id: placa
    resource: /integracoes/raspberry-pi-4.md
---

# Raspberry Pi 4 na porta

## Decisão

- A Orange Pi Zero 2 sai e entra um **Raspberry Pi 4 de 4 GB**.
- O Pi 4 é o **único servidor**: RTSP para o NVR, áudio entre a porta e o telefone, toque, app e
  configuração. Nenhum serviço roda em outro computador.

## Por quê

O usuário quer mais controle. Além disso:
- o Pi 4 tem codificador H.264 por hardware no sistema oficial, o que encerra o problema da
  [análise do H616](/analises/codificacao-h264-no-h616.md);
- tem 4 USB, então a placa de expansão sai;
- tem CPU folgada para o cancelamento de eco.

## Escopo

- Muda a placa da unidade externa e os comandos capturados pelo perfil (`raspi-config`,
  `rpi-eeprom-update`).
- Câmera, Ugreen US205, PAM8403, alto-falante e botão continuam.
- O telefone (ESP32) e as campainhas não mudam.

## Consequências a resolver

- **Tamanho:** 85 × 56 mm contra 53 × 60 mm da Zero 2. Conferir se cabe na carcaça HDL.
- **Servidor no lado da rua:** quem abrir a caixa leva o cartão SD, com a senha do Wi-Fi e as
  credenciais. Mitigar com parafuso de segurança, sem credencial do NVR guardada se não for preciso
  e, se possível, o Pi do lado de dentro do muro.
- **Sem serviço externo:** notificação e acesso fora da rede de casa não vêm prontos. Fora do Wi-Fi,
  é preciso VPN ou túnel (que seria um serviço) ou aceitar que funciona só em casa.

## Como desfazer

Nova `Decisao` e a [ficha da Orange Pi](/integracoes/orange-pi-zero-2.md) volta a valer.
