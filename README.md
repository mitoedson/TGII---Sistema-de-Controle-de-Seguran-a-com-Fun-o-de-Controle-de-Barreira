# Safe Control Systems with Control Barrier Function

Repositório de estudo baseado na tese de doutorado de **Caio Igor Gonçalves Chinelato**,
*"Safe control systems with control barrier function"*, Escola Politécnica da
Universidade de São Paulo (EPUSP), 2022. Orientador: Prof. Dr. Bruno Augusto Angélico.

> Este repositório é um material de apoio pessoal para estudo do conteúdo da tese —
> resumos, explicações e organização das equações e resultados capítulo a capítulo.
> Não substitui a leitura do documento original.

## Sobre a tese

O trabalho apresenta abordagens para o **controle seguro** (*safety-critical control*)
de sistemas dinâmicos usando **Control Barrier Functions (CBFs)**. A ideia central:
todo sistema de controle precisa satisfazer, ao mesmo tempo,

1. **objetivos de estabilidade/rastreamento** (o sistema deve convergir para um ponto
   de equilíbrio ou seguir uma referência), e
2. **restrições de segurança** (o sistema nunca pode entrar em uma região "proibida"
   do espaço de estados).

Esses dois requisitos são unificados em um único controlador através de uma
**programação quadrática (QP)**: quando os dois objetivos entram em conflito, a
segurança sempre tem prioridade.

## Como este repositório está organizado

A estrutura segue exatamente o índice (*Contents*) da tese, um arquivo Markdown por
seção/subseção relevante, para facilitar navegação e revisão isolada de cada tópico.

| # | Capítulo da tese | Arquivo |
|---|---|---|
| 1 | Introduction | [`docs/01-introducao.md`](docs/01-introducao.md) |
| 2 | Control Barrier Function (CBF) | [`docs/02-control-barrier-function.md`](docs/02-control-barrier-function.md) |
| 3 | High Relative-Degree CBF | [`docs/03-high-relative-degree-cbf.md`](docs/03-high-relative-degree-cbf.md) |
| 4 | Robust CBF | [`docs/04-robust-cbf.md`](docs/04-robust-cbf.md) |
| 5 | Discrete-Time CBF (DCBF) | [`docs/05-discrete-time-cbf.md`](docs/05-discrete-time-cbf.md) |
| 6 | Numerical/Experimental Results | [`docs/06-numerical-experimental-results/`](docs/06-numerical-experimental-results/00-overview.md) |
| 7 | Conclusions | [`docs/07-conclusoes.md`](docs/07-conclusoes.md) |
| A–H | Appendices | [`docs/appendices/`](docs/appendices/README.md) |

### Detalhe do Capítulo 6 (o maior, com os experimentos)

| Seção | Sistema | Arquivo |
|---|---|---|
| 6.1 | Pêndulo roda de reação (*Reaction Wheel Pendulum*) | [`01-reaction-wheel-pendulum.md`](docs/06-numerical-experimental-results/01-reaction-wheel-pendulum.md) |
| 6.2 | Pêndulo de Furuta | [`02-furuta-pendulum.md`](docs/06-numerical-experimental-results/02-furuta-pendulum.md) |
| 6.3 | Levitação magnética MIMO (MAGLEV) | [`03-maglev.md`](docs/06-numerical-experimental-results/03-maglev.md) |
| 6.4 | Controle de cruzeiro adaptativo (ACC) automotivo | [`04-acc.md`](docs/06-numerical-experimental-results/04-acc.md) |

### Apêndices

| Apêndice | Conteúdo | Arquivo |
|---|---|---|
| A | Fórmula de controle universal de Sontag | [`A-sontag.md`](docs/appendices/A-sontag.md) |
| B | Reciprocal CBF (RCBF) — a formulação com $B(x)$ | [`B-rcbf.md`](docs/appendices/B-rcbf.md) |
| C | Dedução da solução explícita (Gronwall) | [`C-explicit-solution.md`](docs/appendices/C-explicit-solution.md) |
| D | High Order CBF (HOCBF) | [`D-hocbf.md`](docs/appendices/D-hocbf.md) |
| E | Regulador Linear Quadrático (LQR) | [`E-lqr.md`](docs/appendices/E-lqr.md) |
| F | Controle por Modos Deslizantes (SMC) — MAGLEV | [`F-smc-maglev.md`](docs/appendices/F-smc-maglev.md) |
| G | Controle livre de modelo (*Model-Free Control*) | [`G-model-free-control.md`](docs/appendices/G-model-free-control.md) |
| H | Modelo do veículo (Simulink) | [`H-vehicle-model.md`](docs/appendices/H-vehicle-model.md) |

## Mapa conceitual rápido (a "espinha dorsal" da tese)

```
Capítulo 2 (base)          Capítulo 3              Capítulo 4             Capítulo 5
CBF grau relativo 1   →    Grau relativo alto  →   CBFs robustas     →    Versão discreta
  h(x), QP, CLF             (ECBF, HOCBF)           (RECBF, SMCBF)         (DCBF, NLP)
        │                        │                        │                     │
        └────────────────────────┴────────────────────────┴─────────────────────┘
                                        │
                                        ▼
                        Capítulo 6 — Validação prática
        Pêndulo roda de reação │ Pêndulo de Furuta │ MAGLEV │ ACC automotivo
```

## Contribuições principais da tese

1. **RECBF** (*Robust Exponential Control Barrier Function*) — nova formulação de CBF
   robusta a incertezas de modelo, para restrições de grau relativo alto.
2. **SMCBF** (*Sliding Mode Control Barrier Function*) — nova formulação de CBF robusta
   baseada em modos deslizantes.
3. Aplicação **experimental** (não só numérica) de uma solução explícita (sem QP) para
   restrições de grau relativo alto e robustas.
4. Aplicações práticas inéditas na literatura: pêndulo roda de reação, pêndulo de
   Furuta com CBF, e ACC com controlador de nível superior + nível inferior simultâneos.

## Glossário rápido de notação

| Símbolo | Significado |
|---|---|
| $x$, $D$ | Estado do sistema; domínio onde o estado é definido |
| $u$, $U$ | Entrada de controle; domínio das entradas admissíveis |
| $f(x)$, $g(x)$ | Dinâmica livre e matriz/campo de entrada do sistema afim ($\dot x = f(x)+g(x)u$) |
| $h(x)$ | Zeroing CBF (ZCBF) — função de barreira que vale 0 na fronteira do conjunto seguro |
| $B(x)$ | Reciprocal CBF (RCBF) — função de barreira que tende a infinito na fronteira |
| $C$, $\partial C$, $\text{Int}(C)$ | Conjunto seguro, sua fronteira e seu interior |
| $V(x)$ | Control Lyapunov Function (CLF) |
| $L_f h$, $L_g h$ | Derivadas de Lie de $h$ ao longo de $f$ e de $g$ |
| $\gamma$, $\alpha_h(\cdot)$ | Parâmetro/função de classe $\kappa$ que regula a "agressividade" da CBF |
| QP | Quadratic Programming — programação quadrática, usada para unificar CLF/lei nominal com CBF |
| ECBF | Exponential CBF — trata restrições de grau relativo > 1 |
| RECBF / SMCBF | Versões robustas da ECBF (Capítulo 4) |
| DCBF | Discrete-time CBF (Capítulo 5) |

## Como usar este repositório

- Se está revendo um conceito específico, vá direto ao arquivo correspondente — cada
  um é autocontido e cita a numeração original das equações/definições/teoremas da
  tese entre parênteses, ex.: `(2.13)`, `Definição 2.6`, `Teorema 2.2`.
- Os arquivos do Capítulo 6 têm a estrutura: modelagem → resultado sem CBF → resultado
  com CBF/ECBF/RECBF/SMCBF → conclusão sobre robustez.
- Referências bibliográficas citadas seguem o padrão `(AUTOR, ANO)` da tese original;
  a lista completa de referências está no documento fonte (`References`, p. 121).
