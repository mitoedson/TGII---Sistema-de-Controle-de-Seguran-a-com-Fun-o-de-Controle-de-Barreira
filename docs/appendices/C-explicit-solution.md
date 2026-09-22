# Appendix C — Explicit Solution (dedução via desigualdade de Gronwall)

[⬅ Anterior: Apêndice B](B-rcbf.md)

Dedução da solução explícita (2.19)/(3.10)/(4.21) usada no corpo da tese, mostrada
aqui na formulação original com a RCBF $B(x)$ (Igarashi, Tezuka, Nakamura, 2019).

## Fórmula

$$u_{cbf} = \begin{cases} -\dfrac{I_h(x,u_{no})-J_h(x)}{2}\dfrac{(L_gB(x))^T}{\|L_gB(x)\|} & \text{se } I_h(x,u_{no})>J_h(x) \\[4pt] 0 & \text{caso contrário} \end{cases} \tag{C.1}$$

com $I_h(x,u_{no})=L_fB(x)+L_gB(x)u_{no}$, $J_h(x)=K_hB(x)+C_h$ (C.2).

## Desigualdade de Gronwall

> **Teorema C.1.** Seja $z_h:[t_0,t_1]\to\mathbb{R}_{\geq0}$ absolutamente contínua,
> não negativa, satisfazendo
> $$z_h(t) \leq k_h(t) + \int_{t_0}^t z_h(s_h)v_{he}(s_h)ds_h \tag{C.3}$$
> com $k_h$ não negativa e continuamente diferenciável, $v_{he}$ não negativa e
> contínua. Então, $\forall t\in[t_0,t_1]$:
> $$z_h(t) \leq k_h(t_0)\exp\left(\int_{t_0}^tv_{he}(s_h)ds_h\right) + \int_{t_0}^tk_h'(s_h)\exp\left(\int_{s_h}^tv_{he}(r_h)dr_h\right)ds_h \tag{C.4, C.5}$$

Caso particular ($k_h(t)=K_h(t-t_0)+C_h\geq0$, $v_{he}(x)=L_h\geq0$):

$$z_h(t) \leq L_h + \left(\frac{C_h}{K_h}\right)e^{K_h(t-t_0)} - \frac{C_h}{K_h} \tag{C.6}$$

## Prova de (C.1) — ideia central

$\dot B(x) = L_fB(x)+L_gB(x)u+L_gB(x)u_{no}$ (C.7).

- **Caso 1** ($I_h < J_h$): $\dot B(x) = I_h(x,u_{no}) < J_h(x) = K_hB(x)+C_h$ (C.8),
  logo $\dot B(x)\leq K_hB(x)+C_h$ (C.9) — a barreira cresce de forma controlada.
- **Caso 2** ($I_h > J_h$): substituindo (C.1), obtém-se exatamente
  $\dot B(x)=J_h(x)=K_hB(x)+C_h$ (C.10). Pela desigualdade de Gronwall:
  $$B(t) \leq B(t_0)\exp(K_ht)+\frac{C_h}{K_h}\exp(K_ht)-\frac{C_h}{K_h} \tag{C.11}$$
  — portanto, para qualquer $t\geq0$, $B(x)<\infty$: a barreira **nunca explode**,
  garantindo que o sistema permanece dentro do conjunto seguro.

Esta prova é a base formal que justifica por que a solução explícita (sem QP)
garante segurança — a mesma lógica é reaproveitada nas adaptações para $h(x)$ (2.19),
ECBF (3.10) e SMCBF (4.21) usadas no corpo principal da tese.

---
[⬅ Apêndice B](B-rcbf.md) · [➡ Apêndice D](D-hocbf.md)
