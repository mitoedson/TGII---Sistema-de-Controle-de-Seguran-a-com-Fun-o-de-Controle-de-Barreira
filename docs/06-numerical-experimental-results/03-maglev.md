# 6.3 MIMO MAGLEV System (Levitação Magnética)

[⬅ Voltar à visão geral do Cap. 6](00-overview.md) | [⬅ Anterior: 6.2 Pêndulo de Furuta](02-furuta-pendulum.md)

Primeiro sistema **MIMO** (múltiplas entradas/saídas) e com **múltiplas** restrições
de segurança simultâneas na tese. Compara diretamente **ECBF vs. RECBF vs. SMCBF**
sob incerteza de modelo.

## 6.3.1 System Modeling

Sistemas MAGLEV são usados em mancais magnéticos, plataformas de posicionamento de
alta precisão, trens maglev, etc. Aqui: placa metálica em formato "Y" (alumínio com
peças de ferro nas bordas), levitada por 3 eletroímãs, baseada no aparato
experimental de (Fujii et al., 1994) e (Tsujino, Nakashima, Fujii, 1999).

- Sinais de controle: tensões $V_1,V_2,V_3$ → correntes $i_1,i_2,i_3$ → forças
  atrativas $F_1,F_2,F_3$.
- Saídas: posições da placa $r_1,r_2,r_3$ (medidas por sensores de *gap*).

**Equações de movimento** (vertical, *pitch*, *roll*):

$$M\ddot x_v = Mg-(F_1+F_2+F_3) \tag{6.41}$$
$$J_{pm}\ddot\theta_p = F_1l_{1g}-(F_2+F_3)l_{2g}-Mgd_{ml}\sin\theta_p \tag{6.42}$$
$$J_{rm}\ddot\theta_r = (F_2-F_3)l_{3g}-Mgd_{ml}\sin\theta_r \tag{6.43}$$

Forças eletromagnéticas — **não lineares** nas entradas e nas posições:

$$F_j = k_j\left(\frac{V_j}{r_j}\right)^2, \quad j=1,2,3 \tag{6.47}$$

Sistema MIMO não linear: $\dot x_{ml}=f_{ml}(x_{ml})+g_{ml}(x_{ml})w_{ml}$ (6.48), com
$w_{ml}=[V_1^2\ V_2^2\ V_3^2]^T$.

## 6.3.2 ECBFs

Lei nominal: **SMC** (controle por modos deslizantes clássico, não confundir com
SMCBF — este SMC é para o objetivo de rastreamento, descrito no **Apêndice F**).
Objetivo: as posições $r_j$ devem rastrear referências $r_{jd}$, respeitando limites
$\pm r_{j,max} = r_{j,rs} \pm r_{j,b}$:

$$r_{1,rs}=-0.05\text{m}, \quad r_{2,rs}=-0.07\text{m}, \quad r_{3,rs}=-0.09\text{m}, \quad r_{jb}=0.01\text{m (todos)}$$

**Teste de robustez deliberado:** massa da placa $M$ aumentada em **50%**.

Restrições de grau relativo 2, uma por posição:

$$h_j(x_{ml}) = (r_{jb})^2-(r_j-r_{jrs})^2, \quad j=1,2,3 \tag{6.56}$$

QP com múltiplas ECBFs simultâneas (6.55) — extensão de (3.9) para $m$ restrições em
paralelo. $K_{b_1}=K_{b_2}=[2000\ 200]$, $K_{b_3}=[2000\ 500]$.

### Resultados — ECBFs

| Dinâmica | Resultado |
|---|---|
| Nominal (sem aumento de $M$) | ✅ Respeitada (Fig. 24) |
| Real (massa +50%) | ❌ **Não respeitada** (Fig. 25) |

## 6.3.3 RECBFs

Mesmas restrições, mas com o controlador robusto (4.6) adaptado para 3 restrições
simultâneas (6.57). Mesmos $K_{b_j}$; bounds de incerteza:
$\Delta_{b1_j,max}=0.05$, $\Delta_{b2_j,max}=0.9$.

**Resultado (Fig. 26):** $|r_j|$ nunca excede os limites — **robustamente
satisfeita**, mesmo com a massa real aumentada.

## 6.3.4 SMCBFs

Mesmas restrições, controlador (4.20) adaptado (6.58). Superfície deslizante com
$\lambda_{mls}=\text{diag}(50,50,50)$, $\eta_{mls}=[200\ 200\ 200]^T$,
$\Phi_{mls}=[0.003\ 0.003\ 0.003]^T$, $\Delta_{ml,max}=[10\ 10\ 10]^T$.

**Resultado (Fig. 27):** também **robustamente satisfeita**.

## Comparação final RECBF vs. SMCBF (conclusão do capítulo, adiantada aqui)

Depois de vários testes numéricos, a tese conclui que a **SMCBF apresentou resultados
melhores que a RECBF** em termos de:

- **Precisão**
- **Desempenho**
- **Robustez**
- **Viabilidade** (*feasibility*) do QP

O ponto crítico: em (4.6) (RECBF), os *bounds* $\Delta_{b1,max}$ e $\Delta_{b2,max}$
são parâmetros de projeto com um **trade-off delicado**:
- valores **pequenos demais** → deterioram o desempenho e podem violar a segurança
  (pouca incerteza tolerada);
- valores **grandes demais** → podem tornar o **QP inviável** (sem solução factível).

Com a SMCBF (4.20), **nenhum problema de viabilidade** foi observado nos testes.

---
[➡ Próximo: 6.4 ACC — Veículo Automotivo](04-acc.md)
