# Appendix F — Sliding Mode Control (SMC) - MAGLEV System

[⬅ Anterior: Apêndice E](E-lqr.md)

Detalha o **SMC "clássico"** usado como lei de controle nominal $u_{no}$ (aqui
$w_{no_{ml}}$) para **rastreamento** no sistema MAGLEV (seção 6.3). Importante não
confundir com a **SMCBF** do Capítulo 4 — ali o modo deslizante é aplicado à
*restrição de segurança*; aqui, ao *objetivo de rastreamento*.

## Linearização por derivação da saída

Como a saída $y_{ml}$ não depende diretamente da entrada $w_{ml}$, deriva-se duas
vezes para obter uma relação direta:

$$\ddot y_{ml} = f_{mly}(x_{ml}) + g_{mly}(x_{ml})w_{ml} \tag{F.1}$$

$f_{mly}$ e $g_{mly}$ (matriz $3\times3$) são funções não lineares dos ângulos
$\theta_p,\theta_r$, velocidades angulares e das forças eletromagnéticas — expressões
completas nas equações (F.2)–(F.15) do documento original.

## Superfície deslizante e condição de deslizamento

$$s_{mlc}(y_{ml},t) = \dot{\tilde y}_{ml} + \lambda_{mlc}\tilde y_{ml} \tag{F.16}$$

com $\tilde y_{ml}=y_{ml}-y_{mld}$ (erro de rastreamento), $\lambda_{mlc}$ constantes
positivas.

$$\tfrac{1}{2}\tfrac{d}{dt}s_{mlc}^2 \leq -\eta_{mlc}|s_{mlc}| \tag{F.17}$$

## Controle equivalente + termo robusto

$$\dot s_{mlc} = \ddot y_{ml}-\ddot y_{mld}+\lambda_{mlc}\dot{\tilde y}_{ml} = f_{mly}+g_{mly}w_{ml}-\ddot y_{mld}+\lambda_{mlc}\dot{\tilde y}_{ml} \tag{F.18}$$

Controle equivalente (baseado na dinâmica **nominal** $\bar f_{mly}$, $\bar g_{mly}$):

$$w_{mleq} = \bar g_{mly}^{-1}(-\bar f_{mly}+\ddot y_{mld}-\lambda_{mlc}\dot{\tilde y}_{ml}) \tag{F.19}$$

Lei de controle completa (leva $y_{ml}$ a $y_{mld}$ mesmo com incertezas):

$$w_{ml} = w_{mleq} - \bar g_{mly}^{-1}K_{mlc}\,\text{sgn}(s_{mlc}) \tag{F.20}$$

Ganho $K_{mlc}$ deve satisfazer:

$$K_{mlc} \geq (g_{mly}^{-1}\bar g_{mly})(\eta_{mlc}+f_{mly})-\bar f_{mly}+(I-g_{mly}^{-1}\bar g_{mly})(\ddot y_{mld}-\lambda_{mlc}\dot{\tilde y}_{ml}) \tag{F.21}$$

## Camada limite (anti-*chattering*)

Mesmo princípio do Capítulo 4 — troca-se `sgn` por `sat` com camada limite $\Phi_{mlc}$:

$$w_{ml} = w_{mleq} - \bar g_{mly}^{-1}K_{mlc}\,\text{sat}(s_{mlc}/\Phi_{mlc}) \tag{F.22}$$

Esta é a lei $w_{no_{ml}}$ efetivamente usada nas simulações da seção 6.3 (ECBFs,
RECBFs, SMCBFs) como objetivo de rastreamento.

---
[⬅ Apêndice E](E-lqr.md) · [➡ Apêndice G](G-model-free-control.md)
