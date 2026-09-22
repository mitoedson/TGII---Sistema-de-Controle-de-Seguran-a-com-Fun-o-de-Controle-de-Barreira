# 2. Control Barrier Function (CBF)

[⬅ Voltar ao índice](../README.md) | [⬅ Anterior: Introdução](01-introducao.md)

Este é o capítulo **fundacional** da tese: define o sistema, o conjunto seguro, a CLF,
a CBF e o framework de controle via QP que será estendido nos capítulos seguintes.

## 2.1 Literature Review

### O sistema de controle afim (2.1)

```math
\begin{array}{cl} \dot{x} = f(x) + g(x)u & \text{(2.1)} 
\end{array}
```

- $x \in D \subset \mathbb{R}^n$: estado; $u \in U \subset \mathbb{R}^m$: entrada.
- $f(x)$, $g(x)$ localmente Lipschitz — garante existência e unicidade local de
  soluções (Picard–Lindelöf), condição necessária para toda a análise de invariância
  que segue.
- "Afim no controle" porque $u$ entra linearmente, mesmo com $f$, $g$ não lineares.

### Duas famílias de função de barreira

| | Zeroing CBF — $h(x)$ | Reciprocal CBF — $B(x)$ |
|---|---|---|
| Na fronteira $\partial C$ | $h(x) \to 0$ | $B(x) \to \infty$ |
| Definida em | $D$ inteiro (inclusive fora de $C$, onde é negativa) | apenas em $\text{Int}(C)$ |
| Adotada nesta tese | ✅ (principal) | usada apenas no Apêndice B |

O autor opta por trabalhar com $h(x)$ ao longo de toda a tese porque, segundo
(Ames et al., 2017), valores ilimitados (como $B(x) \to \infty$) são indesejáveis em
implementações embarcadas/tempo real. A formulação equivalente com $B(x)$ é relegada
ao **Apêndice B**.

### O conjunto seguro (2.2)

```math
C = \{x \in D \subset \mathbb{R}^n : h(x) \geq 0\} \tag{2.2} \\

\partial C = \{x \in D \subset \mathbb{R}^n : h(x) = 0\}  \\

\text{Int}(C) = \{x \in D \subset \mathbb{R}^n : h(x) > 0\}
```

$C$ é o conjunto de estados seguros, definido a partir de **uma única** função $h(x)$:
$C = \text{Int}(C) \cup \partial C$ (identidade topológica padrão de interior/fronteira).

### Forward invariance e safety

> **Definição 2.1.** Seja $u$ um controlador de realimentação tal que (2.1) seja
> localmente Lipschitz. Para toda condição inicial $x_0 \in D$ existe um intervalo
> máximo de existência $I(x_0)$ tal que $x(t)$ é a única solução de (2.1) em $I(x_0)$.
> O conjunto $C$ é **forward invariant** se, para todo $x_0 \in C$, $x(t) \in C$ para
> $x(0)=x_0$ e todo $t \in I(x_0)$.

> **Definição 2.2.** O sistema (2.1) é **seguro** em relação a $C$ se $C$ é forward
> invariant.

### Teorema de Nagumo (1942)

> **Teorema 2.1.** Dado $\dot x = f(x)$, $x \in \mathbb{R}^n$, supondo que o conjunto
> seguro $C$ é o *superlevel set* de uma função suave $h$, i.e.
> $C = \{x : h(x) \geq 0\}$, e que $\frac{\partial h}{\partial x}(x) \neq 0$ para todo
> $x$ com $h(x)=0$, então $C$ é invariante se $\dot h(x) \geq 0 \ \forall x \in \partial C$.

Ou seja: a condição de invariância só **precisa** ser checada na fronteira — no
interior não há risco imediato de "vazamento" para fora de $C$.

### Linha histórica resumida

- **Nagumo (1940s)** — condições necessárias/suficientes de invariância na fronteira.
- **Prajna, Jadbabaie, Rantzer (2000s)** — usam Nagumo para provar segurança de
  sistemas não lineares/híbridos.
- **Tee, Ge, Tay (2009)** — *barrier Lyapunov function* $B(x)$, definida positiva,
  tende a infinito ao se aproximar do limite da restrição.
- **Wieland & Allgower (2007)** — primeira definição formal de CBF, combinando a
  fórmula universal de Sontag com $B(x)$.
- **Romdlony & Jayawardhana (2016)** — *control Lyapunov barrier function*: aplica a
  fórmula de Sontag para satisfazer simultaneamente CLF e CBF — mas só funciona quando
  os dois objetivos podem ser atendidos ao mesmo tempo.
- **Freeman & Kokotovic (1996)** — controlador baseado em QP para CLFs (restrição de
  desigualdade obtida via derivada de Lie da CLF).
- **Ames, Grizzle & Tabuada (2014)** — a abordagem central adotada nesta tese: unifica
  CLF e CBF via QP, com **relaxação** tornando a estabilidade uma restrição *soft* e a
  segurança uma restrição *hard*. Diferença chave: os dois objetivos **não** precisam
  ser satisfeitos simultaneamente — a segurança sempre prevalece.
- Condição $\dot B(x) \le 0$ (Tee/Wieland/Romdlony) → relaxada para
  $\dot B(x) \le \gamma/B(x)$ por **Kong et al. (2013)**: exige invariância de apenas
  um *sublevel set* (não todos), tornando o problema menos restritivo.

## 2.2 Control Lyapunov Function (CLF)

> **Definição 2.3 (Derivada de Lie).** Para $V: \mathbb{R}^n \to \mathbb{R}$ suave e
> $f$ campo vetorial suave, $L_f V = \nabla V \cdot f$.

Um controlador $u$ estabiliza (2.1) se satisfaz:

$$\dot V(x) = L_f V(x) + L_g V(x)u \leq -c_V V(x) \tag{2.3}$$

> **Definição 2.4 (ESCLF).** $V(x)$ continuamente diferenciável é uma
> *exponentially stabilizing CLF* se existem $c_1, c_2, c_V > 0$ tais que, para todo $x$:
>
> $$c_1\|x\|^2 \leq V(x) \leq c_2\|x\|^2 \tag{2.4}$$
> $$\inf_{u \in U}\left[L_f V(x) + L_g V(x)u + c_V V(x)\right] \leq 0 \tag{2.5}$$

Define-se o conjunto de controladores admissíveis:

$$K_{clf}(x) = \{u \in U : L_f V(x) + L_g V(x)u + c_V V(x) \leq 0\} \tag{2.6}$$

Qualquer $u(x) \in K_{clf}(x)$ localmente Lipschitz garante estabilização exponencial:

$$u(x) \in K_{clf}(x) \Rightarrow \|x(t)\| \leq \sqrt{\tfrac{c_2}{c_1}} e^{-\tfrac{c_V}{2}t}\|x(0)\| \tag{2.7}$$

### Controlador QP de Freeman-Kokotovic

$$u^*(x) = \underset{u \in \mathbb{R}^m}{\arg\min} \ \tfrac{1}{2}u^Tu \quad \text{s.t.} \ \psi_0(x) + \psi_1(x)^Tu \leq 0 \tag{2.8}$$

com $\psi_0(x) = L_fV(x)+c_VV(x)$, $\psi_1(x) = L_gV(x)^T$ (2.9). Tem **solução
fechada**:

$$u^*(x) = \begin{cases} -\dfrac{\psi_0(x)\psi_1(x)}{\psi_1(x)^T\psi_1(x)} & \text{se } \psi_0(x) > 0 \\ 0 & \text{se } \psi_0(x) \leq 0 \end{cases} \tag{2.10}$$

Aplicado experimentalmente, por exemplo, em locomoção bípede robótica (Galloway et
al., 2015).

## 2.3 CBF — Definição

Condições "tipo Lyapunov" que, se satisfeitas por $B$ ou $h$, garantem invariância de
$C$:

$$\dot B(x) = L_fB(x)+L_gB(x)u \leq \frac{\gamma}{B(x)} \tag{2.11}$$
$$\dot h(x) = L_fh(x)+L_gh(x)u \geq -\gamma h(x) \tag{2.12}$$

> **Definição 2.5 (Função classe $\kappa$).** $\alpha_h : [0,a_\kappa) \to [0,\infty)$
> é classe $\kappa$ se contínua, estritamente crescente e $\alpha_h(0)=0$.

> **Definição 2.6 (ZCBF).** $h(x)$ continuamente diferenciável é uma **ZCBF** definida
> em $D$, com $C \subseteq D$, se existe função de classe $\kappa$ estendida
> $\alpha_h$ tal que:
> $$\sup_{u \in U}\left[L_fh(x) + L_gh(x)u + \alpha_h(h(x))\right] \geq 0, \ \forall x \in D \tag{2.13}$$

Conjunto de controles seguros:

$$K_{zcbf}(x) = \{u \in U : L_fh(x)+L_gh(x)u+\alpha_h(h(x)) \geq 0\} \tag{2.14}$$

> **Corolário 2.1.** Se $h(x)$ é ZCBF para (2.1), qualquer controlador Lipschitz
> contínuo $u: D \to U$ com $u(x) \in K_{zcbf}(x)$ torna $C$ forward invariant.

Tipicamente $\alpha_h(h(x)) = \gamma h(x)$, $\gamma > 0$.

> *Nota:* o conjunto seguro, a ZCBF e a RCBF são definidos considerando apenas os
> estados $x$. Trabalhos como (Huang, Yong, Chen, 2019/2021) definem uma variante
> — *control-dependent barrier function* — em que o conjunto seguro depende também de
> $u$ (referenciado ao final, na lista de publicações do Capítulo 7).

## 2.4 Control Framework

Duas variantes do framework (Fig. 1a e 1b da tese), dependendo de como o objetivo de
estabilidade é expresso.

### 2.4.1 Unificando CLF e CBF via QP

$$u^*(x) = \underset{(u,\delta)\in\mathbb{R}^m\times\mathbb{R}}{\arg\min} \ \tfrac{1}{2}u^TH(x)u + F(x)^Tu$$
$$\text{s.t.} \quad L_fV(x)+L_gV(x)u+c_VV(x) - \delta \leq 0 \tag{2.15}$$
$$L_fh(x)+L_gh(x)u+\alpha_h(h(x)) \geq 0$$

- $\delta$: parâmetro de **relaxação** — torna a estabilidade uma restrição *soft*
  (via peso $p_\delta$ em $H(x)$), mantendo a segurança como restrição *hard*.
- $c_V$: taxa de convergência da estabilização.
- $\gamma$ (em $\alpha_h(h(x))=\gamma h(x)$): quão perto do limite da barreira o CBF
  passa a agir.
- Parâmetros de projeto: $p_\delta$, $c_V$, $\gamma$.

> **Teorema 2.2.** Se $f$, $g$, $\nabla h$, $\nabla V$, $H(x)$ e $F(x)$ são todos
> localmente Lipschitz, então $u^*(x)$ de (2.15) é localmente Lipschitz contínuo em
> $\text{Int}(C)$ — garante existência/unicidade local de solução do sistema em malha
> fechada e aplicabilidade do Corolário 2.1.

### 2.4.2 Unificando lei de controle nominal e CBF via QP

Alternativa mais versátil: em vez de CLF, usa-se qualquer lei nominal $u_{no}$
(PID, LQR, linearização por realimentação...). A ideia: a restrição de segurança
modifica $u_{no}$ **minimamente**, só quando o estado se aproxima da fronteira de $C$.

Minimiza-se o erro $e_u = u_{no}-u$ (2.16); descartando o termo constante de
$\|e_u\|^2$:

$$u^*(x) = \underset{u\in\mathbb{R}^m}{\arg\min}\ u^Tu - 2u_{no}^Tu$$
$$\text{s.t.}\quad L_fh(x)+L_gh(x)u+\alpha_h(h(x)) \geq 0 \tag{2.18}$$

Não precisa de $\delta$ (não há restrição de estabilidade explícita no QP — o objetivo
de rastreamento já está embutido na função custo via $u_{no}$).

> **Teorema 2.3.** Se $u_{no}$ é localmente Lipschitz, (2.1) é controlável em todo o
> espaço restrito e $C \neq \emptyset$, então $u^*(x)$ de (2.18) é Lipschitz contínuo e
> torna $C$ forward invariant.

**Esta é a formulação mais usada ao longo da tese** (nos experimentos do Capítulo 6).

### 2.4.3 Soluções para o problema de QP

- **(Ames et al., 2017)**: solução fechada para (2.15).
- Na prática: `fmincon`/`quadprog` (MATLAB) para simulação.
- Embarcado: **CVXGEN** (Mattingley & Boyd, 2012) — gera código customizado sem
  instalação de software.
- **Procedimento de Hildreth (1957)**: método numérico simples, aplicável tanto em
  simulação quanto embarcado — é o método efetivamente usado nos experimentos desta
  tese.

## 2.5 Explicit Solution (solução explícita, sem QP)

Baseada em (Igarashi, Tezuka, Nakamura, 2019) — controle assistido de segurança com
operação humana. $u_h$ (entrada do operador humano) é tratada aqui como lei nominal
$u_{no}$; $u = u_h + u_{cbf} = u_{no}+u_{cbf}$.

A tese adapta o trabalho original (que usa a RCBF $B(x)$) para a **ZCBF** $h(x)$:

$$u_{cbf} = \begin{cases} -\dfrac{I_h(x,u_{no})-J_h(x)}{2}\dfrac{(L_gh(x))^T}{\|L_gh(x)\|} & \text{se } I_h(x,u_{no}) < J_h(x) \\[4pt] 0 & \text{se } I_h(x,u_{no}) \geq J_h(x) \end{cases} \tag{2.19}$$

com

$$I_h(x,u_{no}) = L_fh(x)+L_gh(x)u_{no}, \qquad J_h(x) = K_hh(x)+C_h \tag{2.20}$$

$K_h$, $C_h$: parâmetros de projeto. Dedução completa via desigualdade de Gronwall no
**Apêndice C**.

**Vantagem:** computacionalmente mais barato — não é preciso resolver o QP a cada
instante de amostragem.
**Desvantagem:** não se aplica a múltiplas CBFs simultâneas.

> Resultados de (Igarashi et al., 2019) eram apenas numéricos; nesta tese são
> verificados **numérica e experimentalmente** (ver seção 6.2).

---
[➡ Próximo: Capítulo 3 — High Relative-Degree CBF](03-high-relative-degree-cbf.md)
