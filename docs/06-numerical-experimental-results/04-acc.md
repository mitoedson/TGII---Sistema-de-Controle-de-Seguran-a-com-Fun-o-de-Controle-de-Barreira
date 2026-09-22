# 6.4 ACC — Adaptive Cruise Control (Automotive Vehicle)

[⬅ Voltar à visão geral do Cap. 6](00-overview.md) | [⬅ Anterior: 6.3 MAGLEV](03-maglev.md)

Aplicação final e mais completa da tese: combina praticamente todos os tópicos —
CLF+CBF contínuo, DCBF discreto, atraso de entrada (Smith predictor) e um controlador
de **dois níveis** (superior + inferior), algo não tratado em conjunto na literatura
anterior sobre ACC + CBF.

## 6.4.1 ACC — Introduction

**ADAS** (*Advanced Driver Assistance Systems*): tecnologias para reduzir acidentes e
melhorar conforto — ACC, *lane keeping*, *collision avoidance* (CA), estacionamento
automatizado.

- **Cruise Control (CC)**: rastreia velocidade de cruzeiro $v_c$ definida pelo
  motorista (só acelerador).
- **ACC**: adapta a velocidade do veículo *host* para manter distância segura a um
  veículo líder — quando não há líder, comporta-se como CC. Usa acelerador **e** freio.
- **CA**: evita colisão com o líder.

Malha de controle em **dois níveis** (Fig. 29):
- **Nível superior** (*outer loop*): calcula a aceleração/desaceleração desejada
  $a_h^*$ para manter distância segura.
- **Nível inferior** (*inner loop*): recebe $a_h$ e $a_h^*$, gera os sinais de
  controle $u_{th}$ (acelerador) e $u_{br}$ (freio) para que $a_h$ rastreie $a_h^*$.

### Restrições de conforto

Segundo (Moon, Moon, Yi, 2009): 98% das acelerações/desacelerações de motoristas reais
ficam entre −2.17 e 1.77 m/s²; desconforto significativo acima de 3–4 m/s²; por
legislação (ISO 15622), desaceleração máxima limitada a 3 m/s². (Naus et al., 2010)
considera *jerk* (derivada da aceleração) máximo de 3 m/s³.

**Formulação do problema:** objetivo de rastreamento (velocidade de cruzeiro) +
restrição de segurança (distância segura) + restrições de conforto (picos de
aceleração/*jerk*) — exatamente o tipo de problema para o qual o framework CLF/lei
nominal + CBF via QP é adequado, já que prioriza segurança automaticamente em caso de
conflito.

> Literatura prévia (Ames et al., 2017; Xu et al., 2017; Mehra et al., 2015) já
> aplicou CBF ao ACC, mas **sem tratar atraso de entrada** e **só considerando o nível
> superior** — as duas lacunas que esta seção preenche.

## 6.4.2 Application 1 — Veículo real da EPUSP (com atraso de entrada)

Veículo real: Volkswagen Polo Sedan 2.0L, ECU (unidade eletrônica de controle)
open-source, testado em dinamômetro inercial (Brugnolli et al., 2019). **Sem freio
eletrônico** — só $u_{th}$ (acelerador) é usado como entrada.

### Identificação do sistema (com atraso)

$$\frac{V_h(s)}{U_{th}(s)} = \frac{0.1788}{s+0.2041}e^{-0.5s} \quad \text{(contínuo)} \tag{6.59}$$
$$\frac{V_h(z)}{U_{th}(z)} = \frac{0.085}{z-0.903}z^{-1} \quad \text{(discreto, } T=0.5\text{s)} \tag{6.60}$$

Atraso de entrada de **0.5s** (1 período de amostragem) — degrada estabilidade e
desempenho.

### 6.4.2.1 Smith Predictor

Estrutura clássica (O. J. Smith, 1957) que desloca o atraso **para fora** da malha de
controle, permitindo que o controlador atue como se a dinâmica não tivesse atraso.
Função de transferência em malha fechada:

$$\frac{Y(s)}{R_e(s)} = \frac{K_{sm}(s)G(s)}{1+K_{sm}(s)[G(s)-G_n(s)e^{-\tau_ds}+G_n(s)]} \tag{6.61}$$

Se o modelo nominal é exato ($G_n(s)e^{-\tau_ds}=G(s)$), o erro de predição é nulo e:

$$\frac{Y(s)}{R_e(s)} = \frac{K_{sm}(s)G(s)}{1+K_{sm}(s)G_n(s)} \tag{6.62}$$

— o atraso é efetivamente compensado, e $K_{sm}(s)$ pode ser projetado como se o
processo não tivesse atraso.

> **Limitação conhecida:** o Smith predictor pressupõe atraso constante e modelo
> exato — é sensível a incertezas de modelo.

### 6.4.2.2 Control Framework

Sistema sem atraso (após compensação):
$x_{acc}=[v_h\ x_r]^T$, $u_{acc}=u_{th}$.

- **CLF** (contínuo): $V_{acc}=e_{acc}^2$ (6.65), com
  $e_{acc}=v_c-(\hat y_{acc}(t+\tau_d)+e_p(t))$ — erro de rastreamento da velocidade,
  usando as predições do Smith predictor.
- **Lei nominal PI** (discreto, algoritmo incremental/de velocidade, reduz problemas
  de precisão numérica):
  $$u_{no_{acc}} = u_{acc_{k-1}} + K_{P_{acc}}(e_{acc_k}-e_{acc_{k-1}}) + K_{I_{acc}}T_{dacc}e_{acc_k} \tag{6.66}$$
- **CBF**: $h_{acc} = x_r - \tau_{th}e_{acc}$ (6.67) — garante distância segura $x_r$
  proporcional ao *time headway* $\tau_{th}$.

Framework contínuo aplicado via (2.15); framework discreto via (5.7). Resultados
apresentados tanto em contínuo (6.4.2.3) quanto em discreto (6.4.2.4) — em ambos os
casos, o objetivo de rastreamento e a restrição de segurança foram satisfeitos, e o
Smith predictor compensou adequadamente o atraso de entrada.

## 6.4.3 Application 2 — Veículo genérico, com controlador de 2 níveis

Aqui a malha completa (Fig. 29) é considerada: **nível superior** + **nível
inferior**, simulados no MATLAB/Simulink com o *Vehicle Dynamics Blockset*.

### 6.4.3.1 Upper Level Controller

$$\dot x_{acc} = \begin{bmatrix}-F_r(v_h)/M_h\\ v_l-v_h\end{bmatrix}+\begin{bmatrix}1\\0\end{bmatrix}u_{acc} \tag{6.68}$$

- **CLF**: $V_{acc}=(v_h-v_c)^2$ (6.69)
- **CBF**: $h_{acc}=x_r-\tau_{th}v_h$ (6.70), $\tau_{th}=1.8$s
- **Restrição de conforto:** $-2\text{m/s}^2 \leq a_h^* \leq 2\text{m/s}^2$
- Framework (2.15), com $F_r$ = arrasto aerodinâmico
  ($F_r = \tfrac{1}{2T_a}C_{ad}A_fP_{abs}(v_h-w_x)^2$)
- Parâmetros: $\gamma_{acc}=1$, $c_{V_{acc}}=5$, $p_{\delta_{acc}}=100$,
  $M_h=1500$kg.

**Resultado (Fig. 40):** $v_h$ atinge $v_c$ respeitando a CLF; ao detectar líder mais
lento, o CBF reduz $v_h$ automaticamente. $h_{acc}$ sempre satisfaz o conjunto seguro.

### 6.4.3.2 Lower Level Controller — Controle livre de modelo (*model-free*)

Diferente de literatura prévia (que usa PID), aqui aplica-se **controle livre de
modelo** (Fliess & Join, 2013) — modelo ultra-local + estimadores algébricos das
dinâmicas desconhecidas (detalhado no **Apêndice G**):

$$\dot u_{th} = \phi_{th}+\alpha_{th}a_h, \qquad \dot u_{br} = \phi_{br}+\alpha_{br}a_h \tag{6.73, 6.74}$$

$$u_{th} = \frac{-\phi_{th}+\dot a_h^*-K_{P_{th}}e_{th}}{\alpha_{th}}, \qquad u_{br} = \frac{-\phi_{br}+\dot a_h^*-K_{P_{br}}e_{br}}{\alpha_{br}} \tag{6.75, 6.76}$$

Zona morta imposta ($|a_h^*|<2\times10^{-5} \Rightarrow u_{th}=u_{br}=0$) para evitar
comutação indesejada entre acelerador e freio; filtro passa-baixa (0.5s) para
suavizar a resposta, tornando-a mais próxima da ação humana.

### 6.4.3.3 Resultados — nível superior + inferior simultâneos

Modelo de veículo complexo (motor + *driveline* + rodas/freios, detalhado no
**Apêndice H**), adaptado do *MathWorks Vehicle Dynamics Blockset* (2021).
$\gamma_{acc}=3$ (ajustado); $\alpha_{th}=0.005$, $K_{P_{th}}=0.6$, $\alpha_{br}=0.1$,
$K_{P_{br}}=10$.

**Resultado (Fig. 42–44):** aceleração $a_h$ rastreia $a_h^*$ com bom desempenho, com
comandos moderados de $u_{th}$/$u_{br}$; restrição de conforto respeitada (pequena
violação pontual em $a_h$, atribuída à ausência de uma trajetória de aceleração em
rampa); CBF $h_{acc}$ sempre satisfaz o conjunto seguro — distância segura garantida
mesmo com o controlador de dois níveis completo em ação.

---
[⬅ Voltar à visão geral do Cap. 6](00-overview.md) ·
[➡ Próximo: Capítulo 7 — Conclusões](../07-conclusoes.md)
