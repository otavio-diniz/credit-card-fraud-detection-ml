# Credit Card Fraud Detection with Machine Learning

Projeto de análise de dados e classificação de fraudes em transações de cartão de crédito, desenvolvido em Python com foco em **dados desbalanceados, comparação de modelos, explicabilidade e validação metodológica**.

> **Contexto acadêmico:** DIO Bootcamp Bradesco — GenAI, Dados & Cyber, Módulo 05 — Análise de Dados com Python, Desafio de Projeto 5.8 — Detecção de Anomalias em Transações em Python.

## Resumo

O projeto investiga como diferentes estratégias de Machine Learning se comportam em uma base extremamente desbalanceada de transações de cartão.

O dataset contém **284.807 transações**, das quais **492 são fraudes** — aproximadamente **0,1727%** da base. Por isso, a análise evita usar acurácia isoladamente como critério de qualidade e prioriza métricas adequadas a eventos raros.

O notebook percorre desde validação e análise exploratória até Regressão Logística, undersampling, Random Forest, XGBoost, ajuste de threshold, Precision-Recall, SHAP e uma análise de sensibilidade sobre duplicatas entre treino e teste.

## Problema analítico

Em detecção de fraude, o custo de um falso negativo e o custo de um falso positivo podem ser muito diferentes. Ao mesmo tempo, a raridade da classe positiva faz com que uma métrica aparentemente alta possa esconder desempenho ruim justamente nos casos mais importantes.

A análise procura responder:

- como comparar modelos em uma base altamente desbalanceada;
- quanto recall adicional custa em falsos positivos;
- como threshold e estratégia de amostragem alteram o comportamento;
- quais features mais influenciam os modelos;
- como duplicatas entre treino e teste afetam a interpretação dos resultados.

## Estado atual

- **Notebook analítico:** materializado em `notebooks/credit_card_fraud_detection.ipynb`.
- **Dataset:** baixado automaticamente pelo notebook; o CSV não é versionado no repositório.
- **Modelos avaliados:** Regressão Logística, undersampling, Random Forest e XGBoost.
- **Explicabilidade:** feature importance e SHAP incluídos.
- **Auditoria metodológica:** duplicatas entre treino/teste investigadas e análise de sensibilidade incorporada à conclusão.
- **Auditoria de prontidão:** realizada em 26/09/2026; consulte [`AUDIT_STATUS.md`](AUDIT_STATUS.md).
- **Reprodutibilidade:** documentada com limitações históricas de versões de dependências; nenhuma versão foi inventada retroativamente.
- **Submissão, nota ou certificação DIO:** não são comprovadas pelo estado deste repositório.

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

Essas conclusões pertencem ao escopo deste dataset, desta metodologia e dos experimentos documentados; não constituem benchmark universal de produção.

## Como reproduzir a análise

### Google Colab

Abra `notebooks/credit_card_fraud_detection.ipynb` no Google Colab e execute as células em ordem.

O notebook baixa o dataset automaticamente. Se o runtime for reiniciado, execute novamente as células anteriores para reconstruir os objetos em memória.

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

Depois abra `notebooks/credit_card_fraud_detection.ipynb`.

## Metodologia

O fluxo analítico documentado inclui:

1. carregamento e validação dos dados;
2. análise de qualidade;
3. análise exploratória;
4. feature engineering;
5. split estratificado e padronização sem leakage pelo scaler;
6. Regressão Logística como baseline;
7. curvas ROC e Precision-Recall;
8. undersampling e ajuste de threshold;
9. Random Forest;
10. XGBoost e GridSearchCV;
11. feature importance e SHAP;
12. testes de integridade, profiling e auditoria de duplicatas;
13. análise de sensibilidade e deliberação técnica final.

### Destaques metodológicos

- `Amount_log = log1p(Amount)` reduziu a assimetria de **16,9777 para 0,1627**.
- O `StandardScaler` foi ajustado **somente no treino**.
- O split 70/30 usa `stratify=y` e `random_state=42`.
- A avaliação prioriza Precision, Recall, F1, ROC-AUC e Average Precision.
- O tuning do XGBoost usa **Average Precision** como `scoring`.
- O SHAP destacou especialmente `V14` e `V4`.
- Foi realizada auditoria de duplicatas entre treino e teste.
- Uma análise de sensibilidade verificou o impacto dessa sobreposição antes da conclusão.

## Dataset

O notebook carrega automaticamente:

`https://storage.googleapis.com/download.tensorflow.org/data/creditcard.csv`

O CSV não é versionado neste repositório.

Variáveis principais:

- `Time`: segundos transcorridos desde a primeira transação;
- `V1`–`V28`: componentes numéricos anonimizados;
- `Amount`: valor da transação, sem moeda assumida;
- `Class`: `0` para normal e `1` para fraude.

## Estrutura e documentação

```text
credit-card-fraud-detection-ml/
├── README.md
├── AUDIT_STATUS.md
├── LICENSE
├── NOTICE.md
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

Documentação complementar:

- [`AUDIT_STATUS.md`](AUDIT_STATUS.md) — matriz de aderência, limitações e baseline auditada.
- [`docs/METODOLOGIA.md`](docs/METODOLOGIA.md) — decisões metodológicas e checkpoints.
- [`docs/RESULTADOS.md`](docs/RESULTADOS.md) — resultados originais e análise de sensibilidade.
- [`docs/COBERTURA_5_8.md`](docs/COBERTURA_5_8.md) — cobertura do conteúdo acadêmico.
- [`docs/PUBLICACAO_GITHUB.md`](docs/PUBLICACAO_GITHUB.md) — histórico e checklist de publicação.
- [`NOTICE.md`](NOTICE.md) — proveniência e fronteiras de direitos de terceiros.

## Dependências e reprodutibilidade

As dependências principais estão em `requirements.txt`:

- NumPy;
- pandas;
- Matplotlib;
- scikit-learn;
- XGBoost;
- SHAP.

As versões exatas do runtime original não foram registradas; o arquivo não fixa versões artificialmente.

As principais etapas aleatórias utilizam `random_state=42`. Tempos de execução não são considerados métricas determinísticas e podem variar conforme o runtime.

A auditoria de 26/09/2026 preservou a limitação histórica de versões de dependências em vez de criar um lockfile retroativo sem evidência. Detalhes em [`AUDIT_STATUS.md`](AUDIT_STATUS.md).

## Limitações

- forte desbalanceamento da classe fraude;
- componentes PCA anonimizados, sem semântica individual de negócio;
- janela temporal curta da base;
- duplicatas de conteúdo no split original;
- ausência de custos financeiros reais para calibrar FP e FN;
- oversampling foi discutido, mas não usado como experimento principal;
- versões exatas do runtime original não foram preservadas.

Para produção ou benchmark formal, o próprio projeto recomenda considerar split por grupos de registros idênticos ou deduplicação controlada antes do treinamento, validação temporal e escolha de threshold orientada por custos reais.

## Contexto acadêmico e reconhecimento

O projeto foi desenvolvido como material de estudo do **Módulo 05 — Análise de Dados com Python** do **DIO Bootcamp Bradesco — GenAI, Dados & Cyber**, no escopo do **Desafio de Projeto 5.8 — Detecção de Anomalias em Transações em Python**.

O desafio foi conduzido por **Isadora D.S. Ferrão (Isadora Ferrão)**. A transcrição do curso registra o convite da instrutora para que os participantes compartilhassem os resultados e a marcassem nas redes sociais. Perfil profissional localizado: <https://br.linkedin.com/in/isadora-ferrao>.

A cobertura acadêmica detalhada está em [`docs/COBERTURA_5_8.md`](docs/COBERTURA_5_8.md).

Agradeço à Isadora, à DIO e ao Bradesco pelo conteúdo e pela oportunidade de aplicar análise de dados, modelagem, desbalanceamento e explicabilidade em um projeto auditável.

Feedback técnico sobre metodologia, tratamento do desbalanceamento, comparação de modelos, SHAP e limitações do experimento é bem-vindo. Como não foi autenticado um GitHub pessoal da instrutora com segurança, **nenhum `@handle` foi inferido**. As menções registram origem acadêmica e reconhecimento e não implicam endosso ou avaliação institucional.

## Autoria, licença e direitos de terceiros

O repositório possui política explícita em [`LICENSE`](LICENSE): **All Rights Reserved** para o material original de Otávio Diniz, sem relicenciamento de dataset, conteúdos de curso, bibliotecas, marcas, publicações ou outros materiais de terceiros.

A disponibilidade pública no GitHub não transforma o projeto em software open source nem concede permissão geral de cópia, modificação, distribuição ou exploração comercial.

Consulte [`NOTICE.md`](NOTICE.md) para proveniência e fronteiras de direitos de terceiros.

## Estado da auditoria

`EVALUATOR_READINESS=PASS_COM_LIMITACOES_DE_REPRODUTIBILIDADE_HISTORICA`

Essa classificação significa que a documentação e as evidências publicadas permitem a um avaliador localizar o notebook, compreender metodologia, resultados, limitações, contexto acadêmico e proveniência. Ela **não** afirma reexecução integral do notebook no ciclo de auditoria, nem comprova submissão, nota ou certificação institucional na DIO.
