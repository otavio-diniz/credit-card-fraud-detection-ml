# Metodologia

## Princípio geral

O projeto foi organizado como um pipeline incremental, com checkpoints que registram evidências novas sem repetir integralmente os blocos anteriores.

## Checkpoint 1 — Entrada e qualidade inicial

- 284.807 linhas e 31 colunas;
- ausência de nulos e infinitos;
- 1.081 duplicatas excedentes;
- nenhuma remoção automática de linhas.

## Checkpoint 2 — Desbalanceamento e EDA

- fraude: 0,1727%;
- razão aproximada: 577,88:1;
- exploração de `Amount`, `Amount = 0`, tempo e correlações;
- acurácia isolada descartada como critério principal.

## Checkpoint 3 — Preparação

- `Amount_log = log1p(Amount)`;
- `X`: `Time`, `V1`–`V28`, `Amount_log`;
- split 70/30 estratificado;
- `StandardScaler` ajustado exclusivamente no treino.

## Checkpoint 4 — Baseline e decisão operacional

Comparação entre:

- Regressão Logística;
- undersampling;
- Random Forest;
- diferentes thresholds.

Conclusão intermediária: recall isolado não é suficiente; falsos positivos e falsos negativos precisam ser avaliados juntos.

## Checkpoint 5 — Modelos avançados e explicabilidade

- XGBoost com `scale_pos_weight`;
- GridSearchCV;
- Average Precision usada como métrica de seleção;
- feature importance;
- SHAP.

## Checkpoint 6 — Validação final

- testes estruturais;
- profiling;
- comparação consolidada;
- auditoria de registros idênticos entre treino e teste;
- análise de sensibilidade removendo da avaliação as linhas sobrepostas.

## Deliberação

A comparação final usa a avaliação de sensibilidade como evidência conservadora.

O Random Forest apresentou o melhor equilíbrio geral por F1 e baixo volume de falsos positivos. O XGBoost ajustado apresentou maior recall e Average Precision, sendo uma alternativa quando maior sensibilidade à fraude é mais importante que o custo adicional de falsos alertas.
