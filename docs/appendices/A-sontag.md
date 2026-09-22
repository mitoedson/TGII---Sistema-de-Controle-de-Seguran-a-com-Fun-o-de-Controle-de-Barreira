# Appendix A — Sontag's Universal Control Formula

[⬅ Voltar ao índice](../../README.md)

Base teórica citada na revisão de literatura (seção 2.1) para as abordagens de
(Wieland & Allgower, 2007) e (Romdlony & Jayawardhana, 2016), que combinam a fórmula
de Sontag com uma função de barreira $B(x)$.

## Condição de pequeno controle (*small-control property*)

Dada uma CLF $V(x)$, o sistema (2.1) tem a propriedade de pequeno controle em relação
a $V(x)$ se para todo $\varepsilon_{clf}>0$ existe $\delta_{clf}>0$ tal que, para todo
$0<\|x\|<\delta_{clf}$, existe $u \in \mathbb{R}^m$ com $\|u\|<\varepsilon_{clf}$ e
$L_fV(x)+L_gV(x)u < 0$.

Intuitivamente: perto da origem, é possível estabilizar o sistema com entradas de
controle arbitrariamente pequenas — evita a necessidade de "esforços" bruscos de
controle na vizinhança do equilíbrio.

## Fórmula universal de Sontag

$$k_{so}(\gamma_{clf},a,b) = \begin{cases} -\dfrac{a+\sqrt{a^2+\gamma_{clf}\|b\|^4}}{b^Tb}b & \text{se } b\neq0 \\ 0 & \text{caso contrário} \end{cases} \tag{A.1}$$

> **Teorema A.1.** Se (2.1) tem CLF $V(x)$ e satisfaz a propriedade de pequeno
> controle, então
> $$u = k_{so}(\gamma_{clf}, L_fV(x), (L_gV(x))^T), \quad \gamma_{clf}>0 \tag{A.2}$$
> é contínua e garante que o sistema em malha fechada é **globalmente
> assintoticamente estável**.

Esta é a base histórica das primeiras formulações de CBF (Apêndice B), embora a tese
opte, na formulação principal, pelo controlador QP baseado em (Freeman & Kokotovic,
1996) em vez da fórmula de Sontag.

---
[⬅ Voltar ao índice de apêndices](README.md) · [➡ Apêndice B](B-rcbf.md)
