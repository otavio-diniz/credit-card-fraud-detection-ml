# Resultados

## Teste original

| Modelo / estratégia | Precision | Recall | F1 | Average Precision | FP | FN |
|---|---:|---:|---:|---:|---:|---:|
| Regressão Logística — baseline | 0,8598 | 0,6216 | 0,7216 | 0,7095 | 15 | 56 |
| Regressão Logística — threshold 0,20 | 0,7863 | 0,6959 | 0,7384 | 0,7095 | 28 | 45 |
| Undersampling | 0,0584 | 0,8784 | 0,1095 | 0,5471 | 2.096 | 18 |
| Random Forest — pesos balanceados | 0,8370 | 0,7635 | 0,7986 | 0,7881 | 22 | 35 |
| XGBoost — inicial | 0,2613 | 0,8581 | 0,4006 | 0,7441 | 359 | 21 |
| XGBoost — GridSearchCV | 0,6557 | 0,8108 | 0,7251 | 0,8158 | 63 | 28 |

## Auditoria de duplicatas

Foram encontrados:

- 377 padrões idênticos presentes simultaneamente em treino e teste;
- 456 linhas do teste com equivalente idêntico no treino;
- dessas 456 linhas, 449 eram classe 0 e 7 eram classe 1.

## Avaliação de sensibilidade

As 456 linhas sobrepostas foram retiradas **somente da avaliação**, preservando os modelos treinados.

Restaram 84.987 registros de teste.

| Modelo / estratégia | Precision | Recall | F1 | Average Precision | FP | FN |
|---|---:|---:|---:|---:|---:|---:|
| Regressão Logística — baseline | 0,8500 | 0,6028 | 0,7054 | 0,6876 | 15 | 56 |
| Regressão Logística — threshold 0,20 | 0,7742 | 0,6809 | 0,7245 | 0,6876 | 28 | 45 |
| Undersampling | 0,0560 | 0,8723 | 0,1052 | 0,5197 | 2.075 | 18 |
| Random Forest — pesos balanceados | 0,8346 | 0,7518 | 0,7910 | 0,7730 | 21 | 35 |
| XGBoost — inicial | 0,2516 | 0,8511 | 0,3883 | 0,7266 | 357 | 21 |
| XGBoost — GridSearchCV | 0,6457 | 0,8014 | 0,7152 | 0,8052 | 62 | 28 |

## Conclusão dos resultados

A retirada da sobreposição reduziu levemente as métricas, mas não alterou materialmente os trade-offs:

- Random Forest: maior F1 e poucos falsos positivos;
- XGBoost ajustado: maior recall e Average Precision;
- undersampling: recall elevado, porém volume de falsos positivos incompatível com uma operação eficiente.

Os sete registros fraudulentos removidos na análise de sensibilidade eram verdadeiros positivos para todos os modelos; os falsos negativos permaneceram inalterados.
