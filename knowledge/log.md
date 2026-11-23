# Log do projeto

## 2026-11-23
* **Descoberta**: H.264 no H616: nenhum kernel oficial codifica por hardware; usar libx264 e deixar h264_v4l2m2m (ports de 2026) como experimento — ver [análise](/analises/codificacao-h264-no-h616.md)
* **Decisão**: microfone e alto-falante da porta pela Ugreen US205 do usuário; NVR aceita RTSP, sem ONVIF — ver [arquitetura](/decisoes/2026-11-23-arquitetura-inicial.md)
* **Descoberta**: imagem Orange Pi 6.1.31 (a mesma da Zero 2): o motor de vídeo só tem o Cedrus, que decodifica; não há codificador por hardware e o x264 já vem — ver [Orange Pi Zero 2](/integracoes/orange-pi-zero-2.md)
* **Descoberta**: manual da Orange Pi Zero 2: sem microfone (nem pela placa de expansão) e sem codec de vídeo no Linux; microfone da porta vai por placa de som USB — ver [Orange Pi Zero 2](/integracoes/orange-pi-zero-2.md)
* **Descoberta**: câmera USB só entrega MJPEG/YUY2: H.264 para o NVR será codificado em software — ver [câmera](/integracoes/camera-usb-2mp.md)
* **Decisão**: arquitetura inicial: Orange Pi na porta, ESP32 no telefone, campainhas D1/S2 mini, servidor RTSP próprio em Go — ver [decisão](/decisoes/2026-11-23-arquitetura-inicial.md)
* **Atualização**: estado com os requisitos e o hardware disponível informados pelo usuário
* **Decisão**: memória no lastro com perfil de código adaptado: tipo Peca, escopo bancada, captura de gravação de firmware e FreeCAD — ver [decisão](/decisoes/2026-11-23-memoria-lastro-perfil-interfone.md)
* **Criação**: memória do projeto montada no padrão lastro (OKF v0.2) — perfil em `.claude/memoria.yaml`.
