# Appendix H — Vehicle Model

[⬅ Anterior: Apêndice G](G-model-free-control.md)

Descreve o bloco **CAR Model** do Simulink (MathWorks, 2021) usado na simulação
completa do ACC com controlador de dois níveis (seção 6.4.3.4, Fig. 41).

## Blocos internos do modelo do veículo

| Bloco | Função |
|---|---|
| **Engine** | Modelo de motor mapeado — tabelas de consulta (*lookup tables*) para potência, vazão de ar, vazão de combustível, temperatura de exaustão, eficiência e emissões |
| **Driveline** | Transmissão idealizada de marcha fixa, sem embreagem/sincronização (*Transmission*) + diferencial como trem de engrenagens planetárias cônicas (*Rear Differential*); troca de marcha implementada como máquina de estados (*Transmission Shift Logic*) |
| **Wheels & Brakes** | Comportamento longitudinal de quatro rodas ideais |
| **Vehicle Dynamics** | Corpo rígido de 1 grau de liberdade (1 DOF), massa constante, movimento longitudinal |

## Observação prática relatada pelo autor

O mecanismo de troca de marcha (implementado via máquina de estados) e o modelo de
embreagem (não totalmente preciso) geram **picos (spikes)** na aceleração do veículo
no momento da troca de marcha. Para atenuar esse efeito espúrio, um **filtro
passa-baixa** foi aplicado — mencionado também na seção 6.4.3.4 do capítulo de
resultados.

Este modelo mais complexo (comparado ao modelo simplificado 1-DOF usado na seção
6.4.3.2, que considera só o bloco *Vehicle Dynamics*) é o que permite testar o
controlador de nível inferior (Apêndice G) em condições mais realistas, incluindo os
efeitos do motor e da transmissão.

---
[⬅ Apêndice G](G-model-free-control.md) · [⬆ Voltar ao índice principal](../../README.md)
