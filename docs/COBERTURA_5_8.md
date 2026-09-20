# Cobertura do conteúdo do Módulo 5.8

A revisão do material do 5.8 indica cobertura dos principais tópicos apresentados no curso.

| Conteúdo | Cobertura no projeto |
|---|---|
| carregamento com pandas | Sim |
| inspeção do dataset | Sim |
| `Time`, `Amount`, `Class`, `V1`–`V28` | Sim |
| desbalanceamento | Sim |
| Precision e Recall | Sim |
| `Amount_log` | Sim |
| `StandardScaler` | Sim |
| definição de `X` e `y` | Sim |
| split 70/30 com `stratify` | Sim |
| Regressão Logística | Sim |
| matriz e métricas | Sim |
| ROC / AUC | Sim |
| Precision-Recall | Sim |
| undersampling | Sim |
| oversampling | Abordado conceitualmente |
| Random Forest | Sim |
| ajuste de threshold | Sim |
| XGBoost | Sim |
| GridSearchCV | Sim |
| feature importance | Sim |
| SHAP | Sim |

## Diferenças deliberadas

### Scaler

O material do projeto ajusta o `StandardScaler` apenas sobre o treino, evitando o uso de estatísticas do teste no `fit`.

### Métrica de tuning

O GridSearchCV usa **Average Precision**, enquanto o exemplo do curso enfatiza recall. A escolha foi motivada pelos resultados do próprio projeto: perseguir recall isoladamente produziu falsos positivos em excesso.

### Pipeline

As etapas são mostradas explicitamente no notebook, em vez de encapsuladas em um objeto `sklearn.pipeline.Pipeline`. A lógica de processamento permanece separada entre treino e teste.

### Oversampling

O conceito é discutido, mas não foi escolhido como experimento principal. Foram avaliados undersampling, pesos de classe, `scale_pos_weight` e threshold.

## Cobertura adicional

O projeto adiciona:

- auditoria de nulos e infinitos;
- investigação detalhada de duplicatas;
- análise de `Amount = 0`;
- exploração temporal;
- correlações;
- Average Precision;
- testes de integridade;
- profiling;
- auditoria de duplicatas entre treino e teste;
- análise de sensibilidade antes da conclusão.

Essas extensões tornam o notebook mais próximo de um fluxo de análise reproduzível do que de uma simples reprodução dos exemplos apresentados.
