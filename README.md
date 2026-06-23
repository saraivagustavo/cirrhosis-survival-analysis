# Previsão de Sobrevivência de Pacientes com Cirrose Hepática via Aprendizado de Máquina

[![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)](https://www.python.org/)
[![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-Latest-orange.svg)](https://scikit-learn.org/)
[![XGBoost](https://img.shields.io/badge/XGBoost-Latest-green.svg)](https://xgboost.readthedocs.io/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Este repositório contém o código-fonte, os experimentos e as análises estatísticas do estudo comparativo entre algoritmos de classificação para predição do quadro clínico de pacientes acometidos por cirrose hepática. O projeto foi desenvolvido como o **Trabalho Prático da disciplina de Inteligência de Negócios (Período 2026/1)** do curso de Bacharelado em Sistemas de Informação no **Instituto Federal do Espírito Santo (Ifes - Campus Serra)**.

O artigo científico completo gerado a partir deste código e estruturado sob as diretrizes da *IEEE Transactions* pode ser consultado no arquivo `Previsão de sobrevivência de pacientes com cirrose usando KNN, Random Forest e XGBoost.docx (1).pdf` presente neste repositório.

---

## Resumo do Projeto

A cirrose hepática consiste no estágio final da fibrose avançada do fígado, caracterizando-se pela perda da arquitetura funcional do órgão. Avaliar a progressão e prognóstico da sobrevida dos pacientes representa um desafio clínico crítico. 

Este trabalho apresenta um estudo comparativo entre três técnicas de classificação do estado da arte em Aprendizado de Máquina: **K-Nearest Neighbors (KNN)**, **Random Forest** e **eXtreme Gradient Boosting (XGBoost)**. Diante do severo desbalanceamento de classes inerente à base de dados médica utilizada (*Cirrhosis Patient Survival Prediction*), o pipeline metodológico avaliou os modelos em dois cenários: com a distribuição desbalanceada original e após a aplicação da técnica de sobreamostragem sintética **SMOTE** (*Synthetic Minority Over-sampling Technique*) aplicada estritamente no conjunto de treino.

Os resultados demonstram que modelos de *ensemble*, em especial o **XGBoost combinado com o SMOTE**, oferecem o melhor balanço preditivo, preservando a acurácia global (71,43%) e elevando sensivelmente a identificação das classes minoritárias (transplante e óbito).

---

## Tecnologias e Dependências

A implementação foi desenvolvida inteiramente em ambiente **Python 3** usando **Jupyter Notebooks** (Google Colab / Anaconda) presente no arquivo `cirrhosis-survival-analysis.ipynb`.

### Bibliotecas Utilizadas:
* **Manipulação e Análise de Dados:** `pandas`, `numpy`
* **Visualização Estatística:** `matplotlib`, `seaborn`
* **Machine Learning:** `scikit-learn`
* **Boosting de Gradiente:** `xgboost`
* **Balanceamento de Dados:** `imbalanced-learn` (`imblearn`)
* **Repositório de Dados:** `ucimlrepo`

---

## Dataset Utilizado

Os dados são provenientes do **UCI Machine Learning Repository**.
* **Nome original:** *Cirrhosis Patient Survival Prediction dataset*
* **Amostras:** 418 pacientes
* **Atributos:** 20 (incluindo dados demográficos e marcadores bioquímicos como Bilirrubina, Albumina, Cobre, etc.)
* **Variável Alvo (`Status`):** Mapeada para classificação multiclasse:
  * `0`: Censurado (Paciente vivo ao final do acompanhamento)
  * `1`: Transplante Hepático
  * `2`: Óbito

---

## Pipeline Metodológico

O projeto segue um fluxo de engenharia de dados e modelagem rigoroso:

1. **Tratamento de Dados Ausentes (Imputação):** Valores nulos numéricos foram preenchidos com a *mediana* e categóricos com a *moda* via `SimpleImputer` para mitigar vieses de *outliers*.
2. **Codificação Categórica:** Transformação de variáveis qualitativas para o formato numérico por meio de *One-Hot Encoding* (`pd.get_dummies`).
3. **Divisão de Amostragem:** Separação do dataset na proporção de **70% para treinamento** e **30% para teste**, conforme `IN-Especificacao do segundo trabalho - 2026-1.pdf`, aplicando **estratificação** (`stratify=y`) para manter a proporção de classes idêntica entre os conjuntos.
4. **Tratamento de Desbalanceamento (SMOTE):** Geração de amostras sintéticas de vizinhança apenas no subset de treino.
5. **Treinamento e Avaliação:** Treinamento comparativo cruzado avaliado via Matrizes de Confusão, Acurácia Global, Precisão, Revocação (*Recall*) e **F1-Score Macro** (métrica primária devido ao desbalanceamento).

---

## Resultados e Discussão

### Resumo Estatístico dos Experimentos

| Algoritmo | Cenário | Acurácia Global | F1-Score Macro |
| :--- | :--- | :---: | :---: |
| **KNN** | Base Original | 59,52% | 0,44 |
| **KNN** | Com SMOTE | 49,20% | 0,44 |
| **Random Forest** | Base Original | 73,81% | 0,57 |
| **Random Forest** | Com SMOTE | 70,63% | 0,57 |
| **XGBoost** | Base Original | 71,43% | 0,54 |
| **XGBoost** | **Com SMOTE** | **71,43%** | **0,59** |

### Principais Conclusões do Artigo:
* **Viés da Base Original:** Na base desbalanceada, os classificadores priorizaram intensamente a classe majoritária (`0`), gerando uma falsa sensação de alta acurácia global, enquanto erravam quase todas as predições de transplante.
* **Efeito SMOTE:** A técnica equilibrou o aprendizado. Embora o KNN e o Random Forest tenham sacrificado parte da sua acurácia geral para conseguir classificar as minorias, o **XGBoost mitigou esse trade-off**, mantendo a acurácia estável e atingindo o maior F1-Score Macro (0,59).

---

## Estrutura do Repositório

```text
├── cirrhosis-survival-analysis.ipynb                             <- Notebook contendo o pipeline completo
├── Previsão de sobrevivência de pacientes com cirrose...pdf      <- Artigo científico formatado em PDF
├── IN-Especificacao do segundo trabalho - 2026-1.pdf             <- Especificação original do projeto
└── README.md                                                     <- Documentação principal do repositório
```

---

## Como Executar o Projeto

1. Clone o repositório:
   ```bash
   git clone https://github.com/seu-usuario/nome-do-repositorio.git
   cd nome-do-repositorio
   ```

2. Abra o arquivo `cirrhosis-survival-analysis.ipynb` no Jupyter Notebook ou envie para o Google Colab para executar as células sequencialmente. As dependências necessárias serão instaladas diretamente na primeira célula do notebook.

---

## Autores

* **Gustavo Saraiva Mariano** - *Discente (Ifes)* - saraivaifes@gmail.com
* **Pedro Renã da Silva Moreira** - *Discente (Ifes)* - pedrorenanmoreira@gmail.com
* **Profª. Kelly Assis de Souza Gazolli** - *Orientadora/Docente (Ifes)* - kasouza@ifes.edu.br

---

## 📄 Licença

Este projeto está sob a licença MIT - consulte o arquivo [LICENSE](LICENSE) para obter mais detalhes.
