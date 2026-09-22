# Appendix G — Model-Free Control

[⬅ Anterior: Apêndice F](F-smc-maglev.md)

Base teórica do **controle livre de modelo** (Fliess & Join, 2013), usado como
controlador de **nível inferior** no ACC (seção 6.4.3.2) — para os sinais de
acelerador $u_{th}$ e freio $u_{br}$.

## Modelo ultra-local

A dinâmica do sistema é aproximada, em uma janela curta de tempo, por um modelo
genérico **ultra-local**:

$$y^{(\nu)} = \phi_{mf} + \alpha_{mf}u \tag{G.1}$$

- $y$: saída do sistema; $\nu$: ordem do sistema;
- $\phi_{mf}$: representa as partes **desconhecidas** da planta (incluindo possíveis
  distúrbios);
- $\alpha_{mf}$: parâmetro constante escolhido pelo projetista (ajustado por
  tentativa e erro até obter bom desempenho em malha fechada).

## Ideia central: cancelar a dinâmica desconhecida

Com estimativas $\hat\alpha_{mf}$ e $\hat\phi_{mf}$ (via **estimador algébrico**), a
dinâmica ultra-local é cancelada e a dinâmica desejada é imposta:

$$u = \frac{-\hat\phi_{mf}+y_d^{(\nu)}-(\text{dinâmica desejada para } e_{mf})}{\hat\alpha_{mf}} \tag{G.2}$$

com $y_d$ = referência (*set-point*), $e_{mf}=y-y_d$.

## Caso de primeira ordem (o usado na tese)

$$\dot y = \phi_{mf}+\alpha_{mf}u \tag{G.5}$$

$$u = \frac{-\phi_{mf}+\dot y_d-K_Pe_{mf}}{\alpha_{mf}} \tag{G.6}$$

## Estimador algébrico de $\phi_{mf}$

Via cálculo operacional (transformada de Laplace + derivadas em $s$), chega-se a uma
expressão de $\phi_{mf}$ como **integral** de sinais passados de $y$ e $u$ em uma
janela de tempo:

$$\phi_{mf} = -\frac{6}{t^3}\int_0^t(t-2\tau_{mf})y(\tau_{mf})\,d\tau_{mf} - \frac{6\alpha_{mf}}{t^3}\int_0^t(t\tau_{mf}-\tau_{mf}^2)u(\tau_{mf})\,d\tau_{mf} \tag{G.10}$$

Na prática, essas integrais são aproximadas numericamente por regra do **trapézio**,
em uma janela $T=NT_s$ (G.11)–(G.14) — é essa forma discretizada que é implementada
computacionalmente.

## Onde é usado na tese

Seção 6.4.3.2/6.4.3.3 — controlador de nível inferior do ACC: um estimador para
$u_{th}$ (6.73) e outro para $u_{br}$ (6.74), com parâmetros $\alpha_{th}=0.005$,
$K_{P_{th}}=0.6$, $\alpha_{br}=0.1$, $K_{P_{br}}=10$, $N=50$, $T_s=0.02$s.

**Vantagem prática:** não exige um modelo físico detalhado da dinâmica do
acelerador/freio — apenas estima "o que não se sabe" em tempo real e compensa.

---
[⬅ Apêndice F](F-smc-maglev.md) · [➡ Apêndice H](H-vehicle-model.md)
