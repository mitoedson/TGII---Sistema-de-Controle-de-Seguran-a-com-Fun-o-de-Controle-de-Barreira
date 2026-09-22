# Appendix D — High Order CBF (HOCBF)

[⬅ Anterior: Apêndice C](C-explicit-solution.md)

Formulação de (Xiao & Belta, 2019), mais geral que a ECBF (Capítulo 3), para tratar
grau relativo alto arbitrário — citada na revisão do Capítulo 3, mas não usada como
formulação principal nos experimentos.

## Construção em série

Para $h(x,t)$ diferenciável $r$ vezes, define-se uma série de funções:

$$\psi_h^0(x,t) := h(x,t)$$
$$\psi_h^1(x,t) := \dot\psi_h^0(x,t) + \alpha_{h1}(\psi_h^0(x,t))$$
$$\vdots$$
$$\psi_h^r(x,t) := \dot\psi_h^{r-1}(x,t) + \alpha_{hr}(\psi_h^{r-1}(x,t)) \tag{D.1}$$

onde $\alpha_{h1},\ldots,\alpha_{hr}$ são funções classe $\kappa$ (Definição 2.5).

E uma série de conjuntos correspondentes:

$$C_h^1(t) := \{x : \psi_h^0(x,t)\geq0\}, \quad C_h^2(t) := \{x:\psi_h^1(x,t)\geq0\}, \quad \ldots \tag{D.2}$$

## Definição formal

> **Definição D.1 (HOCBF).** $h(x,t)$ é uma HOCBF de grau relativo $r$ se existem
> funções classe $\kappa$ diferenciáveis $\alpha_{h1},\ldots,\alpha_{hr}$ tais que:
> $$L_f^rh(x,t)+L_gL_f^{r-1}h(x,t)u + \frac{\partial^rh(x,t)}{\partial t^r} + O_h(h(x,t)) + \alpha_{hr}(\psi_h^{r-1}(x,t)) \geq 0 \tag{D.3}$$
> para todo $(x,t)$ na interseção $C_h^1(t)\cap C_h^2(t)\cap\cdots\cap C_h^r(t)$.

> **Teorema D.1.** Se $x(t_0)$ está na interseção de todos os conjuntos $C_h^i(t_0)$,
> qualquer controlador Lipschitz contínuo que satisfaça (D.3) torna a interseção
> **forward invariant**.

## Relação entre HOCBF e ECBF

> **Observação D.1.** Escolhendo funções classe $\kappa$ lineares
> $\alpha_{hi} := k_{bi}\psi_h^{i-1}(x,t)$, $k_{bi}>0$ (D.5), a formulação HOCBF se
> reduz exatamente à **mesma formulação da ECBF** do Capítulo 3.

Em outras palavras: **ECBF é um caso particular de HOCBF**, com funções classe
$\kappa$ lineares em vez de gerais — a HOCBF permite maior flexibilidade na escolha
dessas funções, ao custo de maior complexidade de projeto.

---
[⬅ Apêndice C](C-explicit-solution.md) · [➡ Apêndice E](E-lqr.md)
