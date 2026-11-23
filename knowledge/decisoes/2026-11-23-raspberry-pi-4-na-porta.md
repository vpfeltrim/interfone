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
  - id: usuario-2
    resource: "Usuário em 2026-11-23: o Pi fica numa caixa do lado de dentro do muro; do lado de fora, carcaça nova impressa em 3D com os componentes básicos; acesso externo pelo Cloudflare (IP do Pi configurado no túnel)"
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

## Posição e acesso (usuário, 2026-11-23)

- **Duas caixas:**
  - **painel externo**, carcaça nova impressa em 3D, com câmera, microfone, alto-falante e botão;
  - **caixa interna**, do lado de dentro do muro, com o Pi 4, a Ugreen, o PAM8403 e a ventoinha.
  O cartão SD e as credenciais ficam fora do alcance da rua.
- **Acesso de fora:** o Pi entra no túnel Cloudflare que já existe. O túnel é infraestrutura de rede,
  não um serviço da aplicação: a aplicação continua toda no Pi.

## Consequências a resolver

- **Cabos pelo muro:** USB da câmera (2.0, até 5 m sem repetidor; o cabo dela tem 50 cm), microfone
  (cabo blindado até a Ugreen), alto-falante (par saindo do PAM8403, nenhum fio ao GND) e botão.
  Falta a distância e a espessura do muro.
- **O túnel só passa HTTP(S)/WebSocket.** RTSP e UDP (WebRTC) não atravessam. O app remoto recebe
  vídeo e áudio por WebSocket/HTTPS. O RTSP fica só na rede local, para o NVR.
- **Hostname público de uma câmera de porta:** proteger com Cloudflare Access e com a autenticação
  do próprio servidor.
- **Notificação no celular fora de casa** sem serviço de terceiros: o próprio Pi mantém o canal
  (ntfy hospedado no Pi, ou app com conexão persistente). Decidir junto com o protocolo.

## Como desfazer

Nova `Decisao` e a [ficha da Orange Pi](/integracoes/orange-pi-zero-2.md) volta a valer.
