# interfone

Interfone Wi-Fi para a casa, reaproveitando a carcaça e peças do interfone HDL antigo: unidade
externa (câmera, alto-falante, microfone, botão), telefone interno HDL modificado na cozinha, app para
PC e celular, vídeo entregue ao NVR da casa e, opcionalmente, campainhas remotas.

## Organização do repositório

| pasta | conteúdo |
|---|---|
| `firmware/` | código das placas (Raspberry Pi 4 na porta, ESP32 no telefone e nas campainhas) — uma subpasta por unidade |
| `software/` | app de PC e celular, serviços |
| `hardware/` | esquemas de ligação, pinagem, placas (KiCad) |
| `mecanica/` | desenhos de suportes e adaptações da carcaça (FreeCAD, STL) |
| `scripts/`, `deploy/` | ferramentas de build, gravação e instalação |
| `knowledge/` | a memória do projeto (lastro) |

Credenciais (Wi-Fi, NVR, tokens) só no `.env` da raiz, fora do git; o agente não o lê.
NVR da casa: já documentado no projeto `~/projetos/nvrcap` (`knowledge/ambiente/nvr.md`).

## Leia primeiro

@knowledge/estado.md

A memória do projeto é o bundle `knowledge/` (OKF v0.2 + perfil em `.claude/memoria.yaml`), mantido
pelo plugin `lastro`. O mapa está em `knowledge/index.md`. Ao iniciar, o hook entrega o resumo:
atividades em andamento, ações não consolidadas, conceitos vencidos e as últimas entradas do log.

## Memória — o contrato

1. **Gravar pela skill `lastro:memoria`**, nunca de memória própria do formato.
2. **Ação que muda o ambiente é capturada pelo hook** e o turno não encerra sem consolidá-la:
   conceito afetado + `lastro log` + `lastro consolidar` (ou `--nada "<motivo>"`).
3. **`verified` é só do revisor humano.** Conceito sem `verified` serve para investigar, não para
   embasar mudança. `stale_after` vencido: reconferir contra o ambiente antes de agir.
4. **Atividade de escopo que exige autorização** (`mudanca`, ou o que o perfil listar em
   `atividade.exigem_autorizacao`) só sai de `backlog` com `mem_autorizacao` apontando para a `Decisao`.
5. **Dado bruto e credencial não entram.** Só conclusão, com `sources`.
6. **Nada se apaga à mão:** `lastro contrair` mede quem depende antes.
7. **Conceito ancorado no código** (`mem_codigo`): ao mudar o comportamento daquele código, atualize o
   conceito no mesmo turno. `lastro ancoras <arquivo>` diz o que a memória afirma sobre ele.
