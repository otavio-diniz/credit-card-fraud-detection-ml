# Auditoria de prontidão para avaliação

**Projeto:** Credit Card Fraud Detection with Machine Learning  
**Programa:** DIO Bootcamp Bradesco — GenAI, Dados & Cyber  
**Módulo:** 05 — Análise de Dados com Python  
**Desafio:** 5.8 — Detecção de Anomalias em Transações em Python  
**Data desta auditoria:** 26/09/2026  
**Baseline auditada:** `main@27d6900968a225daa14c740dc94369af323163db`

## Resultado

`EVALUATOR_READINESS=PASS_COM_LIMITACOES_DE_REPRODUTIBILIDADE_HISTORICA`

A auditoria documental e técnica do repositório confirma aderência ao percurso do desafio 5.8 registrado no material do curso: preparação e exploração dos dados, avaliação apropriada para classe desbalanceada, técnicas de balanceamento/threshold, comparação de modelos e etapa avançada com explicabilidade. Nenhuma alteração no notebook, resultados ou conclusões acadêmicas foi feita neste ciclo.

## Matriz de evidências

| Eixo do desafio | Evidência no repositório | Estado |
|---|---|---|
| Carregamento e exploração do dataset | `notebooks/credit_card_fraud_detection.ipynb`, README e `docs/COBERTURA_5_8.md` | PASS |
| Qualidade, desbalanceamento e EDA | notebook, `docs/METODOLOGIA.md` e `docs/RESULTADOS.md` | PASS |
| Preparação para modelagem | `Amount_log`, split estratificado, scaler ajustado apenas no treino | PASS |
| Avaliação de classificação | Precision, Recall, F1, ROC-AUC, Average Precision, matrizes e curvas | PASS |
| Balanceamento e decisão operacional | undersampling, pesos de classe e ajuste de threshold | PASS |
| Modelos comparados | Regressão Logística, Random Forest e XGBoost | PASS |
| Tuning | GridSearchCV com Average Precision | PASS |
| Explicabilidade | feature importance e SHAP | PASS |
| Validação adicional | testes de integridade, profiling, duplicatas e análise de sensibilidade | PASS |
| Entrega avaliável | notebook, README, `requirements.txt` e documentação complementar | PASS |
| Dataset no repositório | não versionado; obtido externamente no runtime | INTENCIONAL |
| Licenciamento/proveniência | `LICENSE` e `NOTICE.md` | PASS |

## Decisões metodológicas que não são falhas

- O projeto não usa acurácia isoladamente por causa do forte desbalanceamento.
- O `StandardScaler` é ajustado somente no treino para evitar leakage.
- Oversampling é discutido, mas não adotado como experimento principal; foram testadas outras estratégias de desequilíbrio.
- A conclusão final considera uma análise de sensibilidade sem linhas do teste que possuíam equivalentes idênticos no treino.

## Limitações preservadas

1. As versões exatas do runtime histórico não foram registradas. Esta auditoria não fabrica um lockfile retroativo; `requirements.txt` registra as dependências, não um snapshot exato do ambiente original.
2. O dataset é externo e não possui snapshot ou checksum versionado neste repositório.
3. Este ciclo auditou a consistência do código/documentação publicados e das evidências registradas, mas **não executou novamente o notebook completo em um runtime novo**.
4. O repositório não demonstra, por si só, submissão, nota ou certificação institucional na DIO.

## Controle de alterações desta auditoria

Mudanças autorizadas neste ciclo são exclusivamente documentais:

- explicitação de direitos autorais e limites de reutilização;
- registro de proveniência e direitos de terceiros;
- correção da documentação de publicação para refletir que o repositório já é público e agora possui política explícita de direitos;
- inclusão deste gate de auditoria para navegação do avaliador.

`NOTEBOOK_ALTERADO=NAO`  
`RESULTADOS_ALTERADOS=NAO`  
`MODELOS_ALTERADOS=NAO`  
`SUBMISSAO_DIO_INFERIDA=NAO`
