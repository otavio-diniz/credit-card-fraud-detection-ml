# Credit Card Fraud Detection with Machine Learning

Projeto de análise de dados e classificação de fraudes em transações de cartão de crédito, desenvolvido em Python com foco em **dados desbalanceados, comparação de modelos, explicabilidade e validação metodológica**.

## Visão geral

O dataset contém **284.807 transações**, das quais **492 são fraudes** — aproximadamente **0,1727%** da base. Por isso, a análise não usa acurácia isoladamente como critério de qualidade.

O notebook percorre o fluxo completo:

1. carregamento e validação dos dados;
2. análise de qualidade;
3. análise exploratória;
4. feature engineering;
5. split estratificado e padronização sem leakage pelo scaler;
6. Regressão Logística como baseline;
7. ROC e Precision-Recall;
8. undersampling e ajuste de threshold;
9. Random Forest;
10. XGBoost e GridSearchCV;
11. feature importance e SHAP;
12. testes de integridade, profiling e auditoria de duplicatas;
13. análise de sensibilidade e deliberação técnica final.

## Principais resultados

A conclusão utiliza a **avaliação de sensibilidade sem as 456 linhas do teste que possuíam equivalente idêntico no treino**, tornando a comparação final mais conservadora.

| Modelo / estratégia | Precision | Recall | F1 | Average Precision | FP | FN |
|---|---:|---:|---:|---:|---:|---:|
| Regressão Logística — baseline | 0,8500 | 0,6028 | 0,7054 | 0,6876 | 15 | 56 |
| Regressão Logística — threshold 0,20 | 0,7742 | 0,6809 | 0,7245 | 0,6876 | 28 | 45 |
| Undersampling | 0,0560 | 0,8723 | 0,1052 | 0,5197 | 2.075 | 18 |
| Random Forest — pesos balanceados | **0,8346** | 0,7518 | **0,7910** | 0,7730 | **21** | 35 |
| XGBoost — configuração inicial | 0,2516 | 0,8511 | 0,3883 | 0,7266 | 357 | 21 |
| XGBoost — GridSearchCV | 0,6457 | **0,8014** | 0,7152 | **0,8052** | 62 | **28** |

### Interpretação

Não existe um modelo dominante em todas as métricas.

- **Random Forest** apresentou o melhor equilíbrio geral neste conjunto de experimentos: maior F1, precision elevada e baixo volume de falsos positivos.
- **XGBoost ajustado** apresentou maior recall e Average Precision, sendo uma alternativa quando a prioridade é detectar mais fraudes, aceitando maior volume de falsos alertas.
- **Undersampling** elevou fortemente o recall, mas produziu falsos positivos em excesso.

## Dataset

O notebook carrega automaticamente:

`https://storage.googleapis.com/download.tensorflow.org/data/creditcard.csv`

O CSV não é versionado neste repositório.

Variáveis principais:

- `Time`: segundos transcorridos desde a primeira transação;
- `V1`–`V28`: componentes numéricos anonimizados;
- `Amount`: valor da transação, sem moeda assumida;
- `Class`: `0` para normal e `1` para fraude.

## Destaques metodológicos

- `Amount_log = log1p(Amount)` reduziu a assimetria de **16,9777 para 0,1627**.
- O `StandardScaler` foi ajustado **somente no treino**.
- O split 70/30 usa `stratify=y` e `random_state=42`.
- A avaliação prioriza Precision, Recall, F1, ROC-AUC e Average Precision.
- O tuning do XGBoost usa **Average Precision** como `scoring`.
- O SHAP destacou especialmente `V14` e `V4`.
- Foi realizada auditoria de duplicatas entre treino e teste.
- Uma análise de sensibilidade verificou o impacto dessa sobreposição antes da conclusão.

## Estrutura do repositório

```text
credit-card-fraud-detection-ml/
├── README.md
├── requirements.txt
├── .gitignore
├── notebooks/
│   └── credit_card_fraud_detection.ipynb
└── docs/
    ├── METODOLOGIA.md
    ├── RESULTADOS.md
    ├── COBERTURA_5_8.md
    └── PUBLICACAO_GITHUB.md
```

## Como executar

### Google Colab

Abra `notebooks/credit_card_fraud_detection.ipynb` no Google Colab e execute as células em ordem.

O notebook baixa o dataset automaticamente.

> Se o runtime for reiniciado, execute novamente as células anteriores para reconstruir os objetos em memória.

### Ambiente local

```bash
git clone https://github.com/otavio-diniz/credit-card-fraud-detection-ml.git
cd credit-card-fraud-detection-ml

python -m venv .venv
source .venv/bin/activate   # Linux/macOS
# .venv\Scripts\activate  # Windows

pip install -r requirements.txt
jupyter notebook
```

Abra então o notebook em `notebooks/`.

## Dependências

As dependências principais estão em `requirements.txt`:

- NumPy
- pandas
- Matplotlib
- scikit-learn
- XGBoost
- SHAP

As versões exatas do runtime original não foram registradas; o arquivo não fixa versões artificialmente.

## Reprodutibilidade

As principais etapas aleatórias utilizam `random_state=42`.

Os tempos de execução não são considerados métricas determinísticas e podem variar conforme o runtime.

## Limitações

- forte desbalanceamento da classe fraude;
- componentes PCA anonimizados, sem semântica individual de negócio;
- janela temporal curta da base;
- duplicatas de conteúdo no split original;
- ausência de custos financeiros reais para calibrar FP e FN;
- oversampling foi discutido, mas não usado como experimento principal.

Para produção ou benchmark formal, recomenda-se split por grupos de registros idênticos ou deduplicação controlada antes do treinamento, validação temporal e escolha de threshold orientada por custos reais.

## Contexto acadêmico

O projeto foi desenvolvido como material de estudo do **Módulo 05 — Análise de Dados com Python**, consolidando conceitos do exercício de detecção de anomalias em transações e adicionando validações e análises próprias.

A cobertura detalhada está em [`docs/COBERTURA_5_8.md`](docs/COBERTURA_5_8.md).

## Documentação complementar

- [`docs/METODOLOGIA.md`](docs/METODOLOGIA.md) — decisões metodológicas e checkpoints.
- [`docs/RESULTADOS.md`](docs/RESULTADOS.md) — resultados originais e análise de sensibilidade.
- [`docs/COBERTURA_5_8.md`](docs/COBERTURA_5_8.md) — cobertura do conteúdo acadêmico.
- [`docs/PUBLICACAO_GITHUB.md`](docs/PUBLICACAO_GITHUB.md) — roteiro de commit e publicação.

## Licença

Nenhuma licença foi aplicada automaticamente. Para um portfólio aberto, uma licença permissiva como **MIT** é uma opção comum, mas a escolha deve ser feita pelo proprietário do repositório antes da publicação pública.
