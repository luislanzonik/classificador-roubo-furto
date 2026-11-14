# Classificação de ocorrências de roubo e furto de celulares — São Paulo (2025)

Projeto acadêmico que utiliza uma rede neural **MLP (Multi-Layer Perceptron)** para classificar registros de subtração de celulares nas categorias **Roubo** e **Furto**, a partir de características registradas no boletim de ocorrência.

> **Atenção:** o modelo classifica registros de ocorrências já registradas. Ele não prevê onde ou quando crimes acontecerão e não deve ser utilizado como ferramenta de previsão de risco ou decisão policial.

## Aplicação

A aplicação web foi desenvolvida com Streamlit:

[ Acessar o classificador ](https://classificador-roubo-furto-2gxa9cyuq2rbuhkkfalla.streamlit.app/)

## Objetivo

Desenvolver e avaliar modelos MLP capazes de distinguir registros classificados como roubo ou furto de celulares no município de São Paulo, utilizando dados de 2025 disponibilizados pela Secretaria da Segurança Pública do Estado de São Paulo (SSP-SP).

## Base de dados e tratamento

- **Fonte:** base Celulares Subtraídos — SSP-SP, ano de 2025.
- **Arquivo utilizado no notebook:** `CelularesSubtraidos_2025.xlsx`.
- **Aba utilizada:** `CELULAR_2025`.
- **Recorte geográfico:** ocorrências cujo campo `CIDADE` corresponde ao município de São Paulo.
- **Classes consideradas:** Roubo e Furto.
- **Deduplicação:** utilização da chave `NOME_DELEGACIA + ANO_BO + NUM_BO`; boletins com classificações conflitantes são excluídos.

O campo `CIDADE` representa o município onde ocorreu o fato. Ele não deve ser confundido com `NOME_MUNICIPIO`, que identifica o município da delegacia responsável pelo registro.

A planilha original não está incluída neste repositório. É necessário obtê-la na fonte da SSP-SP e carregá-la no Google Colab para executar o notebook integralmente.

## Variáveis utilizadas

O modelo utiliza informações categóricas e numéricas relacionadas ao registro da ocorrência, incluindo:

- Bairro;
- Tipo e subtipo do local;
- Período da ocorrência;
- Mês;
- Dia da semana;
- Hora.

O notebook realiza o tratamento dos dados, o preenchimento de valores ausentes, a codificação das variáveis categóricas e a padronização das variáveis numéricas.

## Modelagem

Foram comparadas três arquiteturas MLP:

| Modelo | Camadas ocultas |
|---|---:|
| MLP_1_camada_32 | 32 |
| MLP_2_camadas_64_32 | 64, 32 |
| MLP_3_camadas_128_64_32 | 128, 64, 32 |

A base é dividida em treinamento, validação e teste, com estratificação das classes. A escolha da arquitetura é feita pelo **F1-score na validação**. O conjunto de teste fica reservado para a avaliação final.

O notebook também apresenta curvas de perda, acurácia de validação interna, comparação das métricas e análise da diferença entre desempenho de treinamento e validação para investigar possíveis sinais de overfitting.

## Resultados

Na execução de referência documentada no notebook anterior, a arquitetura com uma camada oculta de 32 neurônios apresentou o melhor F1-score na validação (aproximadamente **75,94%**).

Na avaliação de teste dessa execução, foram registrados:

| Métrica | Resultado |
|---|---:|
| Acurácia | 78,23% |
| Precisão | 74,46% |
| Recall | 77,08% |
| F1-score | 75,75% |

Esses números são resultados de uma execução específica. Ao executar novamente o notebook, considere os valores efetivamente calculados e apresentados na saída. Não trate os números desta seção como garantia de desempenho em outros dados.

## Estrutura do repositório

- `app.py` — aplicação web em Streamlit.
- `MLP_Roubo_Furto_SP_2025.ipynb` — notebook de análise, tratamento, treinamento e avaliação.
- `modelo_mlp_roubo_furto.joblib` — modelo treinado e informações necessárias para o pré-processamento.
- `requirements.txt` — bibliotecas necessárias para executar a aplicação.

## Como executar o notebook

1. Abra o [Google Colab](https://colab.research.google.com/).
2. Faça upload de `MLP_Roubo_Furto_SP_2025.ipynb`.
3. Disponibilize o arquivo `CelularesSubtraidos_2025.xlsx` quando solicitado pelo notebook.
4. Execute as células em ordem.
5. Ao final, o notebook gera o modelo treinado e a tabela de comparação das arquiteturas.

## Como executar a aplicação localmente

É necessário ter Python instalado. No terminal, dentro da pasta do projeto, execute:

```bash
pip install -r requirements.txt
streamlit run app.py
```

A aplicação deverá abrir no navegador. O arquivo `modelo_mlp_roubo_furto.joblib` precisa estar na mesma pasta de `app.py`.

## Tecnologias

- Python
- Pandas e NumPy
- Scikit-learn
- Joblib
- Matplotlib e Seaborn (análise e visualizações no notebook)
- Streamlit

## Limitações

- A classificação depende da qualidade, do preenchimento e da representatividade dos registros disponíveis.
- O conjunto contém registros de subtração de celulares, não todas as ocorrências criminais.
- A deduplicação e a exclusão de boletins com naturezas conflitantes afetam a composição da base.
- O desempenho medido nos dados de teste não garante o mesmo resultado em dados futuros ou de outras localidades.
- O sistema é uma ferramenta acadêmica de classificação de registros, não substitui dados oficiais, investigação ou avaliação profissional.

## Finalidade

Projeto desenvolvido para fins acadêmicos, com foco em análise de dados, aprendizado de máquina e desenvolvimento de uma aplicação de demonstração.

## Integrantes do grupo

- Carlos Raul Peixoto Marques — RM 567798
- Luís Roberto Lanzoni Kihara Júnior — RM 567406
- Diego Leite Asprino — RM 561662
