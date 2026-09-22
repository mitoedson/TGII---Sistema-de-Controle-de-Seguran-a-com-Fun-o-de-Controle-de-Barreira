# Appendix B — Reciprocal CBF (RCBF)

[⬅ Anterior: Apêndice A](A-sontag.md)

Formulação **equivalente** à ZCBF $h(x)$ usada no corpo da tese, mas com a
**reciprocal barrier function** $B(x)$ — relegada a este apêndice porque
(Ames et al., 2017) observa que valores ilimitados ($B(x)\to\infty$ na fronteira)
são indesejáveis em implementações embarcadas/tempo real.

## Relação entre $B(x)$ e $h(x)$

$$B(x) = -\log\left(\frac{h(x)}{1+h(x)}\right) \tag{B.1} \qquad \text{ou} \qquad B(x) = \frac{1}{h(x)} \tag{B.2}$$

com $\inf_{x\in\text{Int}(C)}B(x) \geq 0$ e $\lim_{x\to\partial C}B(x)=\infty$ (B.3).

## Definição formal

> **Definição B.1 (RCBF).** $B(x): \text{Int}(C)\to\mathbb{R}$ continuamente
> diferenciável é uma RCBF se existem funções classe $\kappa$ $\alpha_1,\alpha_2,\alpha_B$
> tais que, $\forall x\in\text{Int}(C)$:
> $$\frac{1}{\alpha_1(h(x))} \leq B(x) \leq \frac{1}{\alpha_2(h(x))} \tag{B.4}$$
> $$\inf_{u\in U}\left[L_fB(x)+L_gB(x)u-\alpha_B(B(x))\right] \leq 0 \tag{B.5}$$

Conjunto de controles admissíveis:

$$K_{rcbf}(x) = \{u\in U : L_fB(x)+L_gB(x)u-\alpha_B(B(x)) \leq 0\} \tag{B.6}$$

> **Corolário B.1.** Se $B(x)$ é RCBF, qualquer controlador localmente Lipschitz
> $u:\text{Int}(C)\to U$ com $u(x)\in K_{rcbf}(x)$ torna $\text{Int}(C)$ forward
> invariant.

## Controladores equivalentes aos de (2.15) e (2.18)

$$u^*(x) = \underset{u=(u,\delta)}{\arg\min}\ \tfrac{1}{2}u^TH(x)u+F(x)^Tu$$
$$\text{s.t.}\ L_fV(x)+L_gV(x)u+c_VV(x)-\delta\leq0 \tag{B.7}$$
$$L_fB(x)+L_gB(x)u-\alpha_B(B(x))\leq0$$

$$u^*(x) = \underset{u\in\mathbb{R}^m}{\arg\min}\ u^Tu-2u_{no}^Tu \quad \text{s.t.}\ L_fB(x)+L_gB(x)u-\alpha_B(B(x))\leq0 \tag{B.8}$$

Tipicamente $\alpha_B(B(x)) = \gamma/B(x)$, como em (2.11).

---
[⬅ Apêndice A](A-sontag.md) · [➡ Apêndice C](C-explicit-solution.md)
