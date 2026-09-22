# 4. Robust CBF

[⬅ Voltar ao índice](../README.md) | [⬅ Anterior: Cap. 3 — Grau relativo alto](03-high-relative-degree-cbf.md)

> Este capítulo contém as **duas principais contribuições teóricas** da tese: **RECBF**
> e **SMCBF**.

## Por que robustez importa

Se distúrbios e incertezas de modelo não forem considerados, a CBF pode **deixar de
respeitar** a restrição de segurança na prática (isso é exatamente o que se observa
experimentalmente na seção 6.2 do pêndulo de Furuta e na seção 6.3 do MAGLEV).

## Panorama da literatura

| Trabalho | Contribuição |
|---|---|
| (Xu et al., 2015) | Analisa robustez de CBF sob perturbação de modelo via norma $H_\infty$; só **análise**, não projeta controlador robusto |
| (Kolathaya & Ames, 2019), (Jankovic, 2018) | Projeto de controlador robusto considerando **distúrbios** |
| (Nguyen & Sreenath, 2016b, 2020) | Projeto de controlador robusto considerando **incertezas de modelo**, mas só grau relativo 1, com objetivo expresso como CLF |

**Duas propostas originais desta tese:**

1. **RECBF** — extensão de (Nguyen & Sreenath, 2016b/2020) para **grau relativo alto**
   e com objetivo de estabilidade expresso como **lei nominal** (em vez de CLF).
2. **SMCBF** — extensão da metodologia de ECBF (Nguyen & Sreenath, 2016a): em vez de
   alocação de polos no controle virtual, aplica-se **controle por modos deslizantes
   (SMC)** para lidar com incertezas.

## 4.1 Robust Exponential CBF (RECBF)

Considera-se que $f(x)$, $g(x)$ em (2.1) representam a dinâmica **real** (não
exatamente conhecida), enquanto o controlador é projetado com base na dinâmica
**nominal** $\bar f(x)$, $\bar g(x)$.

### Efeito da incerteza na VIOL

$$h^{(r)}(x,\Delta_{b1},\Delta_{b2},\mu_b) = \mu_b + \Delta_{b1} + \Delta_{b2}\mu_b \tag{4.1}$$

$\Delta_{b1}$, $\Delta_{b2}$: termos relacionados à incerteza de modelo. A condição
ECBF (3.9) torna-se:

$$\psi_{0bv} + \psi_{1bv}(\mu_b+\Delta_{b1}+\Delta_{b2}\mu_b) \geq 0 \tag{4.2}$$

com $\psi_{0bv}=K_b\eta_b(x)$, $\psi_{1bv}=1$.

### Versão robusta (incerteza limitada)

Supondo $\Delta_{b1} \leq \Delta_{b1,max}$, $\Delta_{b2} \leq \Delta_{b2,max}$ (4.3),
as restrições robustas tornam-se duas desigualdades (pior caso positivo e negativo):

$$\psi_{0,bv}^{max} + \psi_{1,bv}^{p}(x)\mu_b \geq 0 \tag{4.4}$$
$$\psi_{0,bv}^{max} + \psi_{1,bv}^{n}(x)\mu_b \geq 0 \tag{4.5}$$

com $\psi_{0,bv}^{max} := \max(\psi_{0,bv}^p, \psi_{0,bv}^n)$,
$\psi_{0,bv}^p := \psi_{0bv}+\psi_{1bv}\Delta_{b1,max}$,
$\psi_{0,bv}^n := \psi_{0bv}-\psi_{1bv}\Delta_{b1,max}$,
$\psi_{1,bv}^p := \psi_{1bv}(I+\Delta_{b2,max})$,
$\psi_{1,bv}^n := \psi_{1bv}(I-\Delta_{b2,max})$.

### Controlador QP robusto (RECBF)

$$u^*(x) = \underset{(u,\mu_b)}{\arg\min}\ u^Tu-2u_{no}^Tu$$
$$\text{s.t.}\quad A_r(x)u+b_r(x)=\mu_b \tag{4.6}$$
$$\psi_{0,bv}^{max}+\psi_{1,bv}^p(x)\mu_b \geq 0, \qquad \psi_{0,bv}^{max}+\psi_{1,bv}^n(x)\mu_b \geq 0$$

onde $A_r = L_gL_f^{r-1}h(x)$, $b_r = L_f^rh(x)$. Aplicável a restrições de grau
relativo alto com incerteza de modelo. (Formulação para lei nominal $u_{no}$ —
adaptável para CLF via 3.8.)

## 4.2 Sliding Mode CBF (SMCBF)

Em vez de alocação de polos (como na ECBF), aplica-se **controle por modos
deslizantes (SMC)** ao controle virtual $\mu_b$ da VIOL (3.3), para lidar diretamente
com incertezas de modelo.

### VIOL nominal e efeito da incerteza

$$\bar\mu_b := L_{\bar f}^r h(x) + L_{\bar g}L_{\bar f}^{r-1}h(x)u \tag{4.7}$$
$$h^{(r)}(x,\Delta,\bar\mu_b) = \bar\mu_b + \Delta \tag{4.8}$$

$\Delta$: diferença entre dinâmica real e nominal (incerteza), suposta limitada:
$|\Delta| \leq \Delta_{max}$ (4.9).

### Superfície deslizante

$$s_{cbf}(x,t) = \left(\frac{d}{dt}+\lambda_{cbf}\right)^{r-1}\tilde h \tag{4.10}$$

com $\tilde h = h-h_d$ ($h_d$: valor desejado da CBF; $\lambda_{cbf}>0$). Rastrear
$h=h_d$ equivale a permanecer na superfície $s_{cbf}=0$.

**Condição de deslizamento** (garante invariância da superfície):

$$\tfrac{1}{2}\tfrac{d}{dt}s_{cbf}^2 \leq -\eta_{cbf}|s_{cbf}| \tag{4.11}$$

### Controle equivalente + termo descontínuo

Controle equivalente (baseado na dinâmica nominal, faria $\dot s_{cbf}=0$):

$$\mu_{eq} = h_d^{(r)} - O_{cbf}(\lambda_{cbf},\tilde h) \tag{4.13}$$

Lei de controle completa (leva $h \to h_d$ apesar da incerteza limitada $\Delta$):

$$\mu_b = \mu_{eq} - K_{smc}\,\text{sgn}(s_{cbf}) \tag{4.14}$$

$K_{smc}$ deve satisfazer $K_{smc} \geq \Delta+\eta_{cbf}$ (4.16) para garantir a
condição de deslizamento.

### Chattering e camada limite

O termo `sgn` gera comutação de alta frequência indesejada (**chattering**). Solução
padrão: camada limite (*boundary layer*) $\Phi$ + função saturação no lugar do sinal:

$$\mu_b = \mu_{eq} - K_{smc}\,\text{sat}(s_{cbf}/\Phi) \tag{4.17}$$
$$\text{sat}(s_{cbf}/\Phi) = \begin{cases} s_{cbf}/\Phi & \text{se } |s_{cbf}| \leq \Phi \\ \text{sgn}(s_{cbf}/\Phi) & \text{se } |s_{cbf}| > \Phi \end{cases} \tag{4.18}$$

### Definição formal

> **Definição 4.1 (SMCBF).** $h(x)$ é uma SMCBF se satisfaz:
> $$\sup_{u\in U}\left[L_f^rh(x)+L_gL_f^{r-1}h(x)u - \mu_{eq}+K_{smc}\text{sat}(s_{cbf}/\Phi)\right] \geq 0 \tag{4.19}$$

### Controlador QP (SMCBF)

$$u^*(x) = \underset{(u,\mu_b)}{\arg\min}\ u^Tu-2u_{no}^Tu$$
$$\text{s.t.}\ L_f^rh(x)+L_gL_f^{r-1}h(x)u=\mu_b, \qquad \mu_b \geq \mu_{eq}-K_{smc}\text{sat}(s_{cbf}/\Phi) \tag{4.20}$$

### Solução explícita (SMCBF)

$$u_{cbf} = \begin{cases} -\dfrac{I_e(x,u_{no})-J_e(x)}{2}\dfrac{(L_gL_f^{r-1}h(x))^T}{\|L_gL_f^{r-1}h(x)\|} & \text{se } I_e(x,u_{no}) < J_e(x) \\[4pt] 0 & \text{caso contrário} \end{cases} \tag{4.21}$$

com $I_e(x,u_{no})=L_f^rh(x)+L_gL_f^{r-1}h(x)u_{no}$ e
$J_e(x) = \mu_{eq}-K_{smc}\text{sat}(s_{cbf}/\Phi)+C_e$ (4.22).

Esta é a formulação usada no **pêndulo de Furuta** (seção 6.2.3, via solução
explícita) e no **MAGLEV** (seção 6.3.4, via QP).

## RECBF vs. SMCBF — comparação prática (adiantando a conclusão do Cap. 7)

| | RECBF | SMCBF |
|---|---|---|
| Base | Alocação de polos robustificada (bounds $\Delta_{b1,max}$, $\Delta_{b2,max}$ como parâmetros de projeto) | Modos deslizantes (bound $\Delta_{max}$) |
| Trade-off | Bounds pequenos → viola segurança; bounds grandes → pode tornar o **QP inviável** | Não apresentou problemas de viabilidade nos testes |
| Resultado no MAGLEV (seção 6.3) | Bom, mas inferior | **Melhor** em precisão, desempenho, robustez e viabilidade |

---
[➡ Próximo: Capítulo 5 — Discrete-Time CBF](05-discrete-time-cbf.md)
