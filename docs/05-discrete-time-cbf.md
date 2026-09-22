# 5. Discrete-Time CBF (DCBF)

[⬅ Voltar ao índice](../README.md) | [⬅ Anterior: Cap. 4 — Robust CBF](04-robust-cbf.md)

Extensão do framework (Capítulos 2–4, contínuo) para sistemas em **tempo discreto**,
seguindo (Agrawal & Sreenath, 2017) e (Takano, Oyama, Yamakita, 2018).

Em tempo discreto, os objetivos são unificados via **programação não linear (NLP)**
geral (potencialmente não convexa); sob certas condições, reduz-se a uma **QP
convexa**.

## O sistema em tempo discreto

$$x_{k+1} = f_d(x_k) + g_d(x_k)u_k \tag{5.1}$$

com $x_k \in D_d \subset \mathbb{R}^n$, $u_k \in U_d \subset \mathbb{R}^m$ — análogo
discreto de (2.1).

## 5.1 Discrete-Time CLF (DCLF)

> **Definição 5.1 (DESCLF / DCLF).** $V_d: D_d \to \mathbb{R}$ é uma
> *discrete-time exponentially stabilizing CLF* se:
> 1. Existem $c_{1d}, c_{2d} > 0$ tais que $c_{1d}\|x_k\|^2 \leq V_d(x_k) \leq c_{2d}\|x_k\|^2$ (5.2)
> 2. Existe $u_k: D_d \to U_d$ e $c_{V_d}>0$ tal que
>    $\Delta V_d(x_k,u_k) + c_{V_d}V_d(x_k) \leq 0$ (5.3), onde
>    $\Delta V_d(x_k,u_k) := V_d(x_{k+1})-V_d(x_k)$.

Análogo discreto de (2.8):

$$u_k^* = \underset{u_k \in D_d}{\arg\min}\ u_k^Tu_k \quad \text{s.t.}\ \Delta V_d(x_k,u_k)+c_{V_d}V_d(x_k) \leq 0 \tag{5.4}$$

## 5.2 DCBF — Definição

Conjunto seguro discreto (análogo a 2.2):

$$C_d = \{x_k \in D_d : h_d(x_k) \geq 0\}, \qquad \partial C_d = \{x_k \in D_d : h_d(x_k)=0\} \tag{5.5}$$

> **Definição 5.2 (DCBF).** $h_d(x_k)$ é uma DCBF se:
> 1. $h_d(x_0) \geq 0$ (condição inicial dentro do conjunto seguro);
> 2. $\Delta h_d(x_k,u_k) + \gamma_d h_d(x_k) \geq 0$, $\gamma_d>0$, onde
>    $\Delta h_d(x_k,u_k) := h_d(x_{k+1})-h_d(x_k)$.

Interpretação: $u_k$ mantém $h_d \geq 0$ dado que $h_d(x_0)\geq0$ — ou seja, mantém a
trajetória dentro de $C_d$.

## 5.3 Unificando DCLF e DCBF via NLP

$$u_k^* = \underset{(u_k,\delta_d)}{\arg\min}\ u_k^Tu_k+p_{\delta_d}\delta_d^2$$
$$\text{s.t.}\ \Delta V_d(x_k,u_k)+c_{V_d}V_d(x_k)-\delta_d \leq 0 \tag{5.6}$$
$$\Delta h_d(x_k,u_k)+\gamma_d h_d(x_k) \geq 0$$

Análogo discreto exato de (2.15): $p_{\delta_d}$ = peso da relaxação (soft/hard),
$c_{V_d}$ = taxa de convergência, $\gamma_d$ = agressividade da DCBF.

## 5.4 Unificando lei nominal e DCBF via NLP

$$u_k^* = \underset{u_k \in \mathbb{R}^m}{\arg\min}\ u_k^Tu_k - 2u_{nod}^Tu_k$$
$$\text{s.t.}\ \Delta h_d(x_k,u_k)+\gamma_d h_d(x_k) \geq 0 \tag{5.7}$$

Análogo discreto exato de (2.18); $u_{nod}$ = lei de controle nominal discreta
(ex.: LQR discreto, PI). Não precisa de $\delta_d$ pelo mesmo motivo de (2.18):
o objetivo de rastreamento já está na função custo.

**Esta é a formulação usada no pêndulo roda de reação em tempo discreto** (seção 6.1.3)
e no ACC em tempo discreto (seção 6.4.2.4).

## Observação prática

Quando a DCBF e o sistema discreto são **lineares** (como no caso do pêndulo roda de
reação, seção 6.1.3), a NLP geral (5.6)/(5.7) se reduz exatamente a uma **QP linear**
— computacionalmente mais simples de resolver (ex.: novamente com o procedimento de
Hildreth).

---
[➡ Próximo: Capítulo 6 — Resultados Numéricos/Experimentais](06-numerical-experimental-results/00-overview.md)
