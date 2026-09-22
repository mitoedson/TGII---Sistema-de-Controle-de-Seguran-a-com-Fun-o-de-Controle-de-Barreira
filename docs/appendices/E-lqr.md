# Appendix E — Linear Quadratic Regulator (LQR)

[⬅ Anterior: Apêndice D](D-hocbf.md)

Base teórica do LQR, usado como lei de controle nominal $u_{no}$ em vários
experimentos do Capítulo 6 (pêndulo roda de reação, pêndulo de Furuta).

## E.1 Continuous-Time LQR

Sistema linear $\dot x = A_cx + B_cu$ (E.1). O LQR encontra a matriz de ganhos $K_c$ do
controlador ótimo $u=-K_cx$ (E.2) que minimiza o índice de desempenho:

$$J_c = \int_0^\infty (x^TQ_cx + u^TR_cu)\,dt \tag{E.3}$$

$Q_c$ semidefinida positiva, $R_c$ definida positiva — pesam a importância relativa do
estado $x$ e da entrada $u$ na minimização.

Resolvendo a **equação de Riccati**:

$$A_c^TP_c+P_cA_c-P_cB_cR_c^{-1}B_c^TP_c+Q_c=0 \tag{E.4}$$

se existe $P_c$ definida positiva que a satisfaz, o sistema em malha fechada é
estável, e:

$$K_c = R_c^{-1}B_c^TP_c \tag{E.5}$$

## E.2 Discrete-Time LQR

Sistema discreto $x_{k+1}=G_dx_k+H_du_k$ (E.6), controlador $u_k=-K_dx_k$ (E.7),
minimizando:

$$J_k = \tfrac{1}{2}\sum_{k=0}^\infty (x_k^TQ_dx_k+u_k^TR_du_k) \tag{E.8}$$

Equação de Riccati discreta:

$$P_d = Q_d + G_d^TP_dG_d - G_d^TP_dH_d(R_d+H_d^TP_dH_d)^{-1}H_d^TP_dG_d \tag{E.9}$$

$$K_d = (R_d+H_d^TP_dH_d)^{-1}H_d^TP_dG_d \tag{E.10}$$

## Onde é usado na tese

| Seção | Sistema | Notas |
|---|---|---|
| 6.1.2 | Pêndulo roda de reação (contínuo) | $K_{cr}=[-3.906\ {-0.599}\ {-0.0569}]$ |
| 6.1.3 | Pêndulo roda de reação (discreto) | $K_{dr}=[-3.3908\ {-0.4475}\ {-0.0351}]$ |
| 6.2.2 | Pêndulo de Furuta | LQR com integrador; $K_F=[-0.1812\ {-8.8716}\ {-0.2146}\ {-0.9226}]$ |

Em ambos os casos, o LQR entra na malha como lei de controle nominal $u_{no}$ nos
frameworks (2.18)/(3.9)/(5.7) — o CBF/ECBF/DCBF atua apenas como correção mínima
quando a restrição de segurança está próxima de ser violada.

---
[⬅ Apêndice D](D-hocbf.md) · [➡ Apêndice F](F-smc-maglev.md)
