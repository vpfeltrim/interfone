---
paths:
  # Os mesmos caminhos de `captura.arquivos` (.claude/memoria.yaml), como glob.
  - "firmware/**"
  - "software/**"
  - "scripts/**"
  - "deploy/**"
  - "hardware/**"
  - "mecanica/**"
  - "**/platformio.ini"
  - "**/sdkconfig*"
---

# Mexendo no código ou no desenho — o que vai para a memória

Editar código, firmware ou desenho que o perfil captura vira ação pendente: o turno não termina sem
fechar. A pendência diz qual conceito ancora o arquivo (`mem_codigo`); `lastro ancoras <arquivo>` mostra o mesmo. Fechar é um destes:

- **Mudou responsabilidade, interface ou invariante** de uma parte do software/firmware → atualize o
  `Modulo` que a ancora. Detalhe de implementação não entra: o código já diz. Entra o que ele **não**
  diz: o porquê, o que não pode quebrar, as armadilhas.
- **Mudou um desenho** (esquema de ligação, pinagem, placa, chicote, suporte impresso, adaptação da
  carcaça HDL) → atualize a `Peca` que o ancora: medida de origem, pino, ordem de montagem, o que mudou
  e por quê. Pinagem é contrato entre firmware e hardware: se mudar, o `Modulo` do firmware muda junto.
- **Corrigiu bug** → `Resolucao` (sintoma → causa → correção → teste que impede a volta).
- **Escolheu placa, biblioteca, protocolo ou arquitetura** → `Decisao` com data no nome.
- **Aprendeu o contrato de algo externo** (placa, módulo, biblioteca, protocolo, NVR) → `Integracao`,
  com `stale_after`.
- **Mediu latência, fps, consumo, alcance** → `Baseline` com `mem_janela`.
- **Gravou firmware ou configurou placa** (esptool, idf.py, pio upload, SD do Orange Pi) → `lastro log
  "Gravação" "<o quê, em qual placa, qual versão>"`; se mudou a configuração vigente, o `Componente`.
- Depois: `lastro log "Mudança no código" "<fato> — ver [x](/modulos/x.md)"` (ou `Desenho`) e `lastro consolidar`.
- **Refatoração sem efeito visível, formatação, teste novo** → `lastro consolidar --nada "<motivo>"`.

Credencial (Wi-Fi, NVR, token) nunca vai para fonte nem desenho: mora no `.env` da raiz, e o build
gera o header numa pasta ignorada.

Edição por Bash (sed, heredoc, gerador) também é cobrada (`captura.estado`): o fim do turno compara o
repositório com o início. Commit de código é deliberado, com mensagem que descreve a mudança; o lastro
só commita a memória.
