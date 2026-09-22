# 1. Introdução

[⬅ Voltar ao índice](../README.md)

## Liveness vs. Safety

O capítulo abre com uma distinção conceitual importante, usada em toda a tese:

- **Liveness** ("vivacidade"): propriedade de que coisas *boas* eventualmente
  acontecem. Exemplo: um ponto de equilíbrio assintoticamente estável é alcançado.
  Está matematicamente ligada a uma **CLF** (Control Lyapunov Function) ou a uma lei
  de controle nominal qualquer.
- **Safety** ("segurança"): propriedade de que coisas *ruins* nunca acontecem.
  Está ligada ao conceito de **invariância de conjuntos**: se uma trajetória começa
  dentro de um conjunto invariante, ela nunca alcança o complemento desse conjunto
  (a região "ruim"). Matematicamente relacionada a uma **CBF** (Control Barrier
  Function).

A observação do autor: historicamente, a teoria de controle deu muito mais atenção à
*liveness* (estabilização, rastreamento) do que à *safety* — e é exatamente essa lacuna
que a tese busca preencher.

## Sistemas *safety-critical*

Um sistema é *safety-critical* quando precisa satisfazer **simultaneamente**:
1. um objetivo de controle (estabilidade/rastreamento), e
2. uma restrição de segurança — sendo que a segurança deve ser **priorizada** em caso
   de conflito.

Exemplos motivacionais citados:

- **Segway** (transportador humano de duas rodas): deve rastrear uma referência de
  velocidade/posição (estabilidade) e nunca tombar, ou seja, o ângulo do corpo nunca
  pode ultrapassar um limite (segurança).
- **ACC (Adaptive Cruise Control)** e *lane keeping* em veículos: o veículo rastreia
  uma velocidade de cruzeiro, mas ao detectar um veículo líder mais lento, precisa
  adaptar a velocidade para manter distância segura.
- **Robôs com pernas caminhando sobre terrenos discretos** (ex: pedras de apoio): errar
  o posicionamento do pé por poucos centímetros pode causar uma queda — os pontos de
  apoio funcionam como restrições de segurança rígidas.

## A ideia central do framework de controle

A tese segue a linha de **Ames, Grizzle e Tabuada (2014)**: unificar objetivos de
estabilidade/rastreamento (CLF ou lei de controle nominal) e restrições de segurança
(CBF) através de uma **programação quadrática (QP)**. Quando os dois entram em
conflito, o framework prioriza sempre a segurança.

## 1.1 Objective (Objetivo)

**Objetivo principal:** desenvolver CBFs robustas para restrições de segurança de
**grau relativo alto**.

Objetivos secundários:
- Aplicação **experimental** (não apenas numérica) de uma solução explícita (sem QP)
  para lidar com restrições de grau relativo alto e robustas.
- Explorar aplicações práticas ainda não tratadas na literatura.

## 1.2 Justification (Justificativa)

- CBFs já foram aplicadas com sucesso na literatura, mas ainda carecem de validação em
  mais aplicações práticas para comprovar sua efetividade.
- **Robustez é essencial**: se distúrbios e incertezas de modelo não forem
  considerados, a CBF pode deixar de respeitar a restrição de segurança na prática —
  por isso CBFs robustas precisam ser estudadas, principalmente para restrições de
  grau relativo alto.
- O framework é versátil: aplica-se a sistemas lineares ou não lineares e pode ser
  combinado com qualquer lei de controle nominal (PID, LQR, linearização por
  realimentação, etc.), o que amplia bastante as possibilidades de aplicação.

## 1.3 Contributions (Contribuições)

As contribuições principais listadas pelo autor:

1. Proposição da **RECBF** (*Robust Exponential Control Barrier Function*) para lidar
   com incertezas de modelo — verificada numericamente.
2. Proposição da **SMCBF** (*Sliding Mode Control Barrier Function*) para lidar com
   incertezas de modelo — verificada numérica **e** experimentalmente.
3. Aplicação experimental de uma **solução explícita** (sem QP) para restrições de
   grau relativo alto e robustas.
4. Aplicação do framework ao **ACC** de um veículo automotivo genérico, considerando
   simultaneamente um controlador de **nível superior** (*outer loop*) e um de
   **nível inferior** (*inner loop*) — diferencial em relação à literatura, que
   normalmente só trata do nível superior.

## 1.4 Methodology (Metodologia)

Resumo do percurso metodológico seguido pelo autor:

1. **Revisão de literatura** sobre segurança de sistemas dinâmicos e CBFs — CLF, CBF,
   formulação básica (grau relativo 1), grau relativo alto, CBFs robustas, solução
   explícita sem QP, e CBFs de tempo discreto (DCBFs).
2. **Testes numéricos** com sistemas conhecidos, para entender aspectos práticos de
   implementação.
3. **Primeiros resultados**: pêndulo roda de reação (numérico + experimental,
   seção 6.1) — aplicação inédita na literatura. Durante os experimentos, percebeu-se
   que a robustez é essencial em aplicações experimentais reais (distúrbios e
   incertezas de modelo têm grande influência). Isso motivou o foco subsequente em
   contribuições teóricas/práticas para CBFs robustas → propostas de RECBF e SMCBF, e
   adaptação da solução explícita para lidar com grau relativo alto e robustez.
4. **Novos resultados**: pêndulo de Furuta (experimental, seção 6.2) e sistema MIMO de
   levitação magnética MAGLEV (numérico, seção 6.3), aplicando RECBFs, SMCBFs e a
   solução explícita.
5. **Resultados finais**: ACC aplicado a veículos automotivos (numérico, seção 6.4).
   O framework já havia sido aplicado ao ACC na literatura, mas sem tratar atraso de
   entrada (*input delay*) e sem considerar o controlador de nível inferior — pontos
   que este trabalho endereça.

### Organização da tese (mapa dos capítulos)

- **Capítulo 2** — revisão de literatura, definição de CLF e CBF, formulação básica do
  framework (QP e solução explícita).
- **Capítulos 3, 4 e 5** — CBFs de grau relativo alto, CBFs robustas e DCBFs,
  respectivamente.
- **Capítulo 6** — resultados numéricos/experimentais.
- **Capítulo 7** — conclusões e publicações.

---
[➡ Próximo: Capítulo 2 — Control Barrier Function (CBF)](02-control-barrier-function.md)
