# 6.1 Reaction Wheel Pendulum (Pêndulo roda de reação)

[⬅ Voltar à visão geral do Cap. 6](00-overview.md)

**Aplicação prática inédita na literatura** com o framework de CBF. Objetivo: validar
o framework básico (grau relativo 1), depois grau relativo 2 (ECBF), depois versão
discreta (DCBF) — tudo com **LQR** como lei de controle nominal.

## 6.1.1 System Modeling

Pêndulo invertido balanceado por uma roda de reação (*flywheel*) atuada. Sistema
clássico em pesquisa de controle (não linearidade, robustez, subatuação) — análogo a
lançadores de foguete, Segway, robôs bípedes. Protótipo desenvolvido no LCA-EPUSP.

- $\alpha$: ângulo do pêndulo; $\theta$: ângulo da roda; $\tau$: torque na roda.

**Equações de Lagrange** (método Lagrangiano, coordenadas generalizadas $\alpha,\theta$):

$$(m_pl_{cp}^2+m_rl_p^2+J_p+J_r)\ddot\alpha + J_r\ddot\theta + (m_pgl_{cp}+m_rgl_p)\sin\alpha = -\tau \tag{6.4}$$
$$J_r(\ddot\alpha+\ddot\theta) = \tau \tag{6.5}$$

Motor DC de ímã permanente:

$$\tau = K_{tr}i_{mr} = \frac{K_{tr}}{R_{mr}}(12\,PWM_r - K_{er}\dot\theta) \tag{6.8}$$

Entrada de controle: $PWM_r \in [-1,1]$ (duty-cycle + direção).

**Valores numéricos:** $m_p=0.117$kg, $m_r=0.119$kg, $J_p=6.25\times10^{-4}$kg·m²,
$J_r=9.46\times10^{-4}$kg·m², $l_p=0.143$m, $l_{cp}=0.0987$m, $K_{tr}=0.0601$N·m/V,
$K_{er}=0.1836$V/(rad/s), $R_{mr}=2.44\Omega$.

## 6.1.2 Continuous-Time Results

Ponto de equilíbrio instável na posição vertical (up). Estratégias clássicas da
literatura (PD, alocação de polos, linearização por realimentação, SMC) só tratam
**estabilidade**, sem segurança. Aqui: **LQR** como lei nominal + **CBF** garantindo
que o ângulo $|\alpha|$ nunca ultrapasse um limite.

### Modelo linearizado e LQR

$$\dot x_{cr} = A_{cr}x_{cr}+B_{cr}u_{cr}, \qquad x_{cr}=[\alpha\ \dot\alpha\ \dot\theta]^T,\ u_{cr}=PWM_r \tag{6.9}$$

Com $Q_{cr}=\text{diag}(4.5837, 3.2058, 0.0071)$, $R_{cr}=30$, obtém-se
$K_{cr}=[-3.906\ {-0.599}\ {-0.0569}]$ (6.12). Lei nominal com referência (rastreamento):

$$u_{no_{cr}} = -K_{cr}x_{cr} + K_{cr_{11}}\alpha_{ref} \tag{6.13}$$

### CBF de grau relativo 1

$$h_{cr}(x_{cr}) = c_{1cr}\left[\alpha_{max}^2-\alpha^2\right] - c_{2cr}\dot\alpha^2 \tag{6.14}$$

- $c_{2cr}\dot\alpha^2$ pondera a influência da velocidade — se pequeno, $h_{cr}\geq0$
  ocorre essencialmente quando $|\alpha| < \alpha_{max}$.
- Parâmetros: $\alpha_{max}=0.087$rad (5°), $c_{1cr}=0.5$, $c_{2cr}=0.001$,
  $\gamma_{cr}=55$ (simulação) / $\gamma_{cr}=1$ (experimento, mais conservador).
- Pulsos de referência: $\pm0.140$rad (±8°), duração 0.2s.

**Efeito de $\gamma_{cr}$:** valor alto → CBF age perto do limite $\alpha_{max}$ (pode
violar a restrição na prática); valor baixo → CBF mais conservadora, age longe do
limite.

**Resultados (Figs. 5–8):**
- Só LQR: estabiliza bem, mas **não respeita** a restrição de segurança.
- LQR + CBF (simulação, $\gamma_{cr}=55$): $|\alpha|$ nunca excede $\alpha_{max}$;
  $h_{cr}(x)$ respeita o conjunto seguro.
- LQR + CBF (experimento, $\gamma_{cr}=1$, mais conservador — necessário por
  imprecisão de sensores, dinâmica não modelada e estimadores de velocidade angular):
  restrição respeitada após o transiente inicial (CBF ativada só após 5s).

### 6.1.2.1 ECBF (grau relativo 2)

Mesmo problema, mas com restrição de **grau relativo 2** (sem o termo de velocidade):

$$h_{cr}(x_{cr}) = \alpha_{max}^2-\alpha^2 \tag{6.15}$$

Aplica-se o framework ECBF (3.9), com $K_{b_{cr}}=[2000\ 100]$. Resultado (Fig. 9):
restrição respeitada — como esperado, já que ECBF é uma **generalização** da CBF de
grau relativo 1 (Observação 3.1), os resultados são muito similares ao caso 6.14.

## 6.1.3 Discrete-Time Results

Modelo linearizado discretizado ($T_{dr}=0.02$s):

$$x_{dr_{k+1}} = G_{dr}x_{dr_k}+H_{dr}u_{dr_k} \tag{6.16}$$

LQR discreto: $Q_{dr}=\text{diag}(0.0057, 0.0040, 0.0001)$, $R_{dr}=0.28$ →
$K_{dr}=[-3.3908\ {-0.4475}\ {-0.0351}]$ (6.19).

DCBF linear (por partes, dependendo do sinal de $\alpha_k$):

$$h_{dr_k}(x_{dr_k}) = \begin{cases}\alpha_{max}-\alpha_k & \alpha_k \geq 0\\ \alpha_{max}+\alpha_k & \alpha_k < 0\end{cases} \tag{6.21}$$

Como a DCBF e o sistema discreto são **lineares**, a NLP geral (5.7) reduz-se
exatamente a uma **QP** (6.23), resolvida também via Hildreth.

Parâmetros: $\alpha_{max}=0.087$rad, $\gamma_{dr}=0.25$ (simulação) /
$\gamma_{dr}=0.1$ (experimento, mais conservador).

**Resultado:** restrição de segurança respeitada tanto em simulação quanto no
protótipo real. Os resultados em tempo discreto foram **melhores** que em tempo
contínuo — atribuído ao fato de que o efeito do *zero-order hold* já está incluso no
modelo discreto (6.16), tornando-o mais fiel à implementação prática real.

> **Nota histórica:** os trabalhos de referência sobre DCBF (Agrawal & Sreenath, 2017;
> Takano, Oyama, Yamakita, 2018) só apresentam validação **numérica**. Aqui, pela
> primeira vez, o DCBF é validado também **experimentalmente**.

---
[➡ Próximo: 6.2 Pêndulo de Furuta](02-furuta-pendulum.md)
