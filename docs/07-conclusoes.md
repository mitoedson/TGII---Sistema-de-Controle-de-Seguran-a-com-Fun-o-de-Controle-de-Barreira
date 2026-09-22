# 7. Conclusions

[⬅ Voltar ao índice](../README.md) | [⬅ Anterior: Cap. 6 — Resultados](06-numerical-experimental-results/00-overview.md)

## Síntese geral

O framework de controle desenvolvido (CLF ou lei nominal + CBF, via QP) foi validado
em quatro plataformas distintas, cobrindo todos os tópicos teóricos dos Capítulos 2–5.
Em **todos os casos**, os objetivos de estabilidade/rastreamento e as restrições de
segurança foram satisfeitos — confirmando a efetividade, versatilidade e robustez do
framework.

## Resultado por sistema

### Pêndulo roda de reação (6.1)
- CBF grau relativo 1 e ECBF (grau relativo 2): restrição satisfeita em ambos os
  casos — resultados muito similares, como esperado (ECBF generaliza CBF, Observação
  3.1).
- Tempo discreto (DCBF): restrição satisfeita, com validação **experimental** inédita
  (literatura prévia só validava numericamente).

### Pêndulo de Furuta (6.2)
- ECBF: satisfaz a restrição **só com dinâmica nominal**; falha sob incerteza de
  modelo real.
- SMCBF: satisfaz a restrição mesmo sob incerteza — resolve o problema.
- Ambos aplicados via **solução explícita** (sem QP); resultados numéricos e
  experimentais similares aos obtidos via QP → confirma que a solução explícita é uma
  alternativa computacionalmente vantajosa (não precisa resolver QP a cada
  amostragem), embora não se aplique a múltiplas CBFs simultâneas.

### MAGLEV MIMO (6.3)
- ECBFs: falham sob incerteza de massa (+50%).
- RECBFs e SMCBFs: ambas corrigem o problema — mas **SMCBF supera RECBF** em
  precisão, desempenho, robustez e viabilidade do QP.
- Trade-off identificado na RECBF: os *bounds* de incerteza ($\Delta_{b1,max}$,
  $\Delta_{b2,max}$) são parâmetros de projeto sensíveis — pequenos demais degradam
  segurança/desempenho, grandes demais podem inviabilizar o QP. Esse trade-off **não
  foi observado** com a SMCBF.

### ACC automotivo (6.4)
- Aplicação 1 (veículo real, EPUSP): Smith predictor compensou adequadamente o
  atraso de entrada; resultados satisfatórios em contínuo e discreto.
- Aplicação 2 (veículo genérico, 2 níveis): tanto o controlador de nível superior
  (CLF+CBF) quanto o de nível inferior (controle livre de modelo) satisfizeram seus
  objetivos simultaneamente — diferencial em relação à literatura prévia, que só
  tratava o nível superior.

## Conclusão central sobre as contribuições

1. As novas formulações de CBFs robustas (**RECBF** e **SMCBF**) garantiram a
   segurança dos sistemas dinâmicos mesmo sob incertezas de modelo.
2. A **solução explícita** (sem QP) apresentou resultados similares ao framework
   baseado em QP, com vantagem computacional (não resolve QP a cada amostragem), mas
   com a limitação de não suportar múltiplas CBFs simultâneas.
3. O framework se mostrou **efetivo, versátil e robusto** em todas as plataformas
   testadas — lineares e não lineares, SISO e MIMO, contínuas e discretas.

## Trabalhos futuros sugeridos

- Resultados numéricos/experimentais com um **helicóptero 2 DOF** (protótipo já
  disponível no LCA-EPUSP; resultados numéricos com ECBFs e SMCBFs já obtidos —
  faltando validação experimental).
- Resultados **experimentais** de ACC em veículos reais da EPUSP (aplicações 1 e 2,
  seção 6.4), complementando os resultados numéricos já obtidos.
- Integração de CBFs com **controle livre de modelo** (Apêndice G) — as derivadas de
  Lie da CBF calculadas a partir da dinâmica ultra-local estimada. Resultados
  numéricos preliminares já obtidos para o helicóptero 2 DOF.

## 7.1 Publications

Lista de publicações decorrentes da tese (resumo — consulte o documento original,
p. 118–120, para a lista completa com todos os dados bibliográficos):

| Publicação | Veículo | Relacionada a |
|---|---|---|
| Safe Adaptive Cruise Control with CBF and Smith Predictor | XXIII CBA (2020) | Seção 6.4.2.3 |
| Control of a MIMO Magnetic Levitation System using ECBF | XXIII CBA (2020) | ECBFs no MAGLEV (6.3) |
| Safe Control of a Reaction Wheel Pendulum Using CBF | IEEE Access (2020) | Seção 6.1 |
| Robust Exponential Control Barrier Functions for Safety-Critical Control | ACC 2021 | RECBF (4.1) — Furuta + MAGLEV |
| Safe Control of a 2DOF Helicopter Using ECBF | SBAI 2021 | Helicóptero 2 DOF (trabalho futuro) |
| A Sliding Mode Approach for High Relative-Degree CBF | Asian J. of Control (em revisão) | SMCBF (4.2) — MAGLEV |
| Practical Application of Robust CBF Using Sliding Modes | Int. J. of Control (em revisão) | Seção 6.2 (Furuta) |
| Design of ACC with CBF and Model-Free Control | J. of Control, Automation and Electrical Systems (em revisão) | Seção 6.4.3.4 |
| Vehicle Lateral Stability Regions for Control Applications | J. of Intelligent Transportation Systems (em revisão, coautoria) | *Control-dependent barrier functions* aplicadas à estabilidade lateral veicular |

---
[➡ Ver apêndices](appendices/README.md)
