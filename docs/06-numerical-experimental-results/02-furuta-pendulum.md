# 6.2 Furuta Pendulum (Pêndulo de Furuta)

[⬅ Voltar à visão geral do Cap. 6](00-overview.md) | [⬅ Anterior: 6.1 Pêndulo roda de reação](01-reaction-wheel-pendulum.md)

**Aplicação prática inédita na literatura.** Aqui a tese demonstra, pela primeira vez
de forma **experimental**, por que CBFs robustas (SMCBF) são necessárias: a ECBF
"comum" falha quando há incerteza de modelo real.

## 6.2.1 System Modeling

Pêndulo de Furuta (Furuta, Yamakita, Kobayashi, 1992): sistema não linear, 2 graus de
liberdade — um braço gira no plano horizontal e um pêndulo gira no plano vertical.
Único atuador: torque no braço. Analogia com robôs caminhantes e propulsores de
foguete. Protótipo desenvolvido no LCA-EPUSP.

- $\theta_{0F}$: ângulo do braço; $\theta_{1F}$: ângulo do pêndulo; $\tau_{mF}$:
  torque no braço.

Equações de Lagrange com energia cinética do braço $K_{0F}$ (6.27), energia cinética
do pêndulo $K_{1F}$ (6.29, com termos de acoplamento entre $\dot\theta_{0F}$ e
$\dot\theta_{1F}$) e energia potencial $V_F=m_{1F}gl_{1F}\cos(\theta_{1F})$ (6.26).
Motor DC análogo ao do pêndulo roda de reação (6.31)–(6.33).

**Valores numéricos:** $m_{0F}=0.393$kg, $m_{1F}=0.068$kg, $2l_{0F}=0.365$m,
$2l_{1F}=0.207$m, $r_F=0.210$m, $d_F=0.022$m, $K_{tF}=0.02$N·m/A, $R_{mF}=2.4\Omega$.

## 6.2.2 ECBF — Explicit Solution

O pêndulo de Furuta tem três objetivos clássicos: *swing-up*, estabilização e
rastreamento de trajetória. A literatura (SMC, redes neurais, linearização,
*fuzzy*) trata só estabilidade/rastreamento, **sem** restrições de segurança.

### Setup experimental

- LQR como lei nominal (com **integrador**, para que $\theta_{0F}$ rastreie a
  referência).
- Modelo linearizado (6.36): $x_F=[\theta_{0F}\ \theta_{1F}\ \dot\theta_{0F}\ \dot\theta_{1F}]^T$.
- $Q_F=\text{diag}(32.8281, 131.3123, 1.3131, 8.2070)$, $R_F=1000$ →
  $K_F=[-0.1812\ {-8.8716}\ {-0.2146}\ {-0.9226}]$ (6.39).
- Referência: $\theta_{1F,ref}=0$ com pulsos $\pm0.052$rad (±3°), duração 0.2s.
- **Restrição de segurança:** $|\theta_{1F}|$ nunca excede
  $\theta_{1F,max}=0.061$rad (3.5°).
- **Teste de robustez deliberado:** a massa do braço $m_{0F}$ é aumentada em
  **65.14%** para verificar a robustez do framework frente a um erro de modelo real.
- Controlador aplicado via **solução explícita** (3.10), não via QP:
  $u_F = u_{no_F}+u_{cbf_F}$.

### Restrição (grau relativo 2)

$$h_F(x_F) = \theta_{1F,max}^2 - \theta_{1F}^2 \tag{6.40}$$

$K_{b_F}=[1100\ 50]$, $C_{e_F}=0$.

### Resultados — ECBF

| Dinâmica considerada | Resultado |
|---|---|
| **Nominal** (sem aumento de massa) | ✅ Restrição respeitada — $\|\theta_{1F}\|$ nunca excede o limite (Fig. 16) |
| **Real** (massa aumentada 65.14%) | ❌ **Restrição violada** — $\|\theta_{1F}\|$ excede o limite (Fig. 17) |

**Conclusão intermediária:** a ECBF **não é robustamente satisfeita** — confirma
experimentalmente a motivação do Capítulo 4.

## 6.2.3 SMCBF — Explicit Solution

Mesma restrição (6.40), mas agora com $u_{cbf_F}$ dado pela **solução explícita da
SMCBF** (4.21). Parâmetros de projeto: $\lambda_{cbf_F}=10$, $\eta_{cbf_F}=5$,
$\Phi_F=0.1$ (camada limite), $\Delta_{F_{max}}=1$ (bound de incerteza, escolhido
empiricamente para aumentar a robustez do controlador).

### Resultados — SMCBF

| Dinâmica considerada | Resultado |
|---|---|
| **Nominal** | ✅ Restrição respeitada (Fig. 18) |
| **Real** (massa aumentada 65.14%) | ✅ **Restrição respeitada** (Fig. 19) |

**Conclusão:** com a SMCBF, a restrição de segurança é **robustamente satisfeita** —
mesmo com o erro de modelo deliberadamente introduzido. Esse é o primeiro resultado
**experimental** que valida a proposta de SMCBF da tese.

---
[➡ Próximo: 6.3 MAGLEV](03-maglev.md)
