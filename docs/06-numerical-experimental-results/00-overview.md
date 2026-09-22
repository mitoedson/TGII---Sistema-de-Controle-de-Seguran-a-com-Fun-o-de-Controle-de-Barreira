# 6. Numerical/Experimental Results — Visão geral

[⬅ Voltar ao índice](../../README.md) | [⬅ Anterior: Cap. 5 — DCBF](../05-discrete-time-cbf.md)

Este capítulo valida experimentalmente/numericamente **todos** os tópicos tratados
nos capítulos 2 a 5, em quatro plataformas diferentes. Os experimentos foram
organizados propositalmente para cobrir progressivamente cada tópico teórico e cada
contribuição da tese.

| Seção | Sistema | O que é testado | Verificação |
|---|---|---|---|
| [6.1](01-reaction-wheel-pendulum.md) | Pêndulo roda de reação | CBF grau relativo 1, ECBF (grau relativo 2), DCBF | Numérica **e** experimental |
| [6.2](02-furuta-pendulum.md) | Pêndulo de Furuta | ECBF vs. SMCBF (robustez), solução explícita | Experimental |
| [6.3](03-maglev.md) | MAGLEV (MIMO) | ECBFs múltiplas, RECBFs, SMCBFs (robustez) | Numérica |
| [6.4](04-acc.md) | ACC automotivo | CLF+CBF (contínuo/discreto), Smith predictor, nível superior+inferior | Numérica |

## Progressão lógica dos experimentos

```
6.1 Pêndulo roda de reação
    └─ CBF grau 1 → ECBF (grau 2) → DCBF
    └─ Aqui se percebe experimentalmente que robustez é crítica
           │
           ▼
6.2 Pêndulo de Furuta
    └─ ECBF falha sob incerteza de modelo → SMCBF corrige (robustez confirmada)
    └─ Usa solução explícita (sem QP)
           │
           ▼
6.3 MAGLEV (MIMO, múltiplas restrições)
    └─ ECBFs falham sob incerteza → RECBFs e SMCBFs corrigem
    └─ Comparação direta RECBF vs. SMCBF
           │
           ▼
6.4 ACC automotivo
    └─ Aplicação prática completa: Smith predictor (atraso), CLF+CBF,
       DCBF, controlador de 2 níveis (superior + inferior)
```

## Infraestrutura experimental comum

Os experimentos com hardware real (6.1 e 6.2) foram conduzidos no
**Laboratório de Controle Aplicado (LCA) da EPUSP**, com:
- Placa de desenvolvimento **Teensy 3.2** (microcontrolador ARM Cortex-M4 32 bits,
  256 KB flash, 64 KB RAM);
- Motores DC de ímã permanente 12V com driver *H-bridge* **VNH5019**;
- Encoders incrementais para medição angular; velocidades obtidas por aproximação de
  Euler *backward*;
- QP resolvida embarcada via **procedimento de Hildreth (1957)**.

---
Navegue pelas seções: [6.1 Pêndulo roda de reação](01-reaction-wheel-pendulum.md) ·
[6.2 Pêndulo de Furuta](02-furuta-pendulum.md) · [6.3 MAGLEV](03-maglev.md) ·
[6.4 ACC](04-acc.md)
