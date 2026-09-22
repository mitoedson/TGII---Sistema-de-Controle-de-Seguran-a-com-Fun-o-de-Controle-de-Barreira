# 3. High Relative-Degree CBF

[⬅ Voltar ao índice](../README.md) | [⬅ Anterior: Cap. 2 — CBF](02-control-barrier-function.md)

## O problema

> **Definição 3.1 (Grau relativo).** O grau relativo de $h(x)$ em relação ao sistema
> (2.1) é o número de vezes que é preciso derivá-la ao longo da dinâmica até que $u$
> apareça explicitamente.

O framework do Capítulo 2 só funciona para restrições de **grau relativo 1**
(quando $L_gh(x) \neq 0$ já na primeira derivada). Em muitos sistemas reais, a
restrição de segurança tem **grau relativo > 1** — nesse caso $L_gh(x) = 0$ e o QP
(2.15)/(2.18) simplesmente **não pode ser montado**, porque $u$ não aparece na
restrição.

## Soluções na literatura

| Trabalho | Ideia | Limitação |
|---|---|---|
| (Wu & Screenath, 2015) | Para restrição de grau relativo 2, $g_b(x)$: define $h(x,\dot x) = \gamma_b g_b(x) + \dot g_b(x,\dot x)$ (3.1) — nova CBF de grau relativo 1 | Só funciona para grau relativo **exatamente 2** |
| (Hsu, Xu, Ames, 2015) | *Backstepping* para grau relativo arbitrário | — |
| **(Nguyen & Sreenath, 2016a)** | **ECBF** — linearização por realimentação virtual entrada-saída (VIOL) + alocação de polos | Adotada e adaptada nesta tese |
| (Xiao & Belta, 2019) | **HOCBF** — formulação mais geral e simples, via funções classe $\kappa$ em série; relacionável à ECBF | Descrita no Apêndice D |

## 3.1 Exponential CBF (ECBF)

Na formulação original (Nguyen & Sreenath, 2016a), o objetivo de estabilidade é uma
CLF e a ECBF é derivada a partir da RCBF $B(x)$. **Nesta tese**, adapta-se para:
objetivo de estabilidade como **lei nominal** $u_{no}$ (como em 2.18), e ECBF derivada
a partir da **ZCBF** $h(x)$ (seguindo Ames et al., 2019).

> O termo "exponencial" vem do fato de que a restrição resultante da CBF é uma função
> exponencial da condição inicial.

### Por que não dá simplesmente para generalizar (2.18)?

Para $\dot h(x,u) = L_fh(x)+L_gh(x)u$, seria necessário aplicar $(L_gh(x))^{-1}$ — mas
$L_gh(x)$ é um **vetor** quando $m>1$ em (2.1), logo **não é invertível**. A solução:
**VIOL** (*virtual input-output linearization*) — não exige matriz de desacoplamento
invertível.

### Linearização por realimentação virtual (VIOL)

Define-se a entrada de controle virtual $\mu_b$:

$$h^{(r)}(x,u) = L_f^rh(x) + L_gL_f^{r-1}h(x)u := \mu_b \tag{3.2}$$

O sistema linearizado por entrada-saída se torna:

$$\dot\eta_b(x) = F_b\eta_b(x) + G_b\mu_b, \qquad h(x) = C_b\eta_b(x) \tag{3.3}$$

onde $\eta_b(x) = [h(x), \dot h(x), \ddot h(x), \ldots, h^{(r-1)}(x)]^T$ (3.4), $F_b$ e
$G_b$ em forma canônica de integradores (3.5), e $C_b = [1\ 0\ \cdots\ 0]$ (3.6).

### Alocação de polos

Para levar $h(x) \to 0$: projeta-se $\mu_b = -K_b\eta_b$ com todos os polos $p_b$ reais
e negativos. Assim, $h(x(t)) = C_be^{A_bt}\eta_b(x_0)$, com $A_b=F_b-G_bK_b$ e todos os
autovalores negativos. Se $\mu_b \geq -K_b\eta_b$, então
$h(x(t)) \geq C_be^{A_bt}\eta_b(x_0)$.

> **Definição 3.2 (ECBF).** Dado $C$ (2.2) para $h(x)$ diferenciável $r$ vezes, $h(x)$
> é uma ECBF se existe $K_b \in \mathbb{R}^r$ tal que, para (2.1):
> $$\sup_{u\in U}\left[L_f^rh(x)+L_gL_f^{r-1}h(x)u+K_b\eta_b(x)\right] \geq 0 \tag{3.7}$$
> $\forall x \in \text{Int}(C)$ implica $h(x(t)) \geq C_be^{A_bt}\eta_b(x_0) \geq 0$
> sempre que $h(x_0) \geq 0$.

### Controladores QP com ECBF

**Com CLF** (estende 2.15):
$$u^*(x) = \underset{(u,\mu_b,\delta)}{\arg\min}\ \tfrac{1}{2}u^TH(x)u+p_\delta\delta^2$$
$$\text{s.t.}\ L_fV(x)+L_gV(x)u+c_VV(x)-\delta \leq 0 \tag{3.8}$$
$$L_f^rh(x)+L_gL_f^{r-1}h(x)u = \mu_b, \qquad \mu_b \geq -K_b\eta_b(x)$$

**Com lei nominal** (estende 2.18 — a forma efetivamente usada nos experimentos):
$$u^*(x) = \underset{(u,\mu_b)}{\arg\min}\ u^Tu-2u_{no}^Tu$$
$$\text{s.t.}\ L_f^rh(x)+L_gL_f^{r-1}h(x)u=\mu_b, \qquad \mu_b \geq -K_b\eta_b(x) \tag{3.9}$$

> **Observação 3.1.** Quando $r=1$, $K_b\eta_b(x)$ reduz-se a $\gamma h(x)$ — a
> Definição 2.6 é o caso particular de grau relativo 1 da Definição 3.2. Ou seja, a
> **ECBF generaliza a CBF** para grau relativo arbitrário.

### Solução explícita adaptada para ECBF

$$u_{cbf} = \begin{cases} -\dfrac{I_e(x,u_{no})-J_e(x)}{2}\dfrac{(L_gL_f^{r-1}h(x))^T}{\|L_gL_f^{r-1}h(x)\|} & \text{se } I_e(x,u_{no}) < J_e(x) \\[4pt] 0 & \text{caso contrário} \end{cases} \tag{3.10}$$

com $I_e(x,u_{no}) = L_f^rh(x)+L_gL_f^{r-1}h(x)u_{no}$ e
$J_e(x)=-K_b\eta_b(x)+C_e$ (3.11), $C_e$ parâmetro de projeto.

Esta é a formulação usada no **pêndulo de Furuta** (seção 6.2.2) e na **base do
RECBF/SMCBF** do Capítulo 4.

---
[➡ Próximo: Capítulo 4 — Robust CBF](04-robust-cbf.md)
