# Análise Exploratória de Dados e Modelagem Inicial de Churn de Clientes

## Introdução

Este notebook documenta o processo de Análise Exploratória de Dados (AED) e a modelagem inicial do *churn* (cancelamento de serviço) de clientes para uma empresa de telecomunicações. O objetivo principal é entender os fatores que levam os clientes a cancelar seus serviços e construir um modelo preditivo básico para identificar clientes em risco.

A evasão de clientes é um desafio significativo para empresas de telecomunicações, impactando diretamente a receita e o crescimento. Compreender os motivos do *churn* e ser capaz de prever quais clientes são mais propensos a sair permite que a empresa implemente estratégias de retenção direcionadas e eficazes.

## Metodologia

A análise seguiu as seguintes etapas:

1.  **Extração de Dados:** Os dados foram carregados a partir de um arquivo JSON (`TelecomX_Data.json`).
2.  **Transformação de Dados:**
    *   Dados aninhados no JSON foram normalizados e integrados ao DataFrame principal.
    *   A coluna `Charges.Total` foi convertida para tipo numérico, tratando valores não convertíveis como ausentes.
    *   A coluna `customerID` foi removida, pois não é relevante para a modelagem preditiva.
    *   Variáveis categóricas foram convertidas para um formato numérico usando One-Hot Encoding.
3.  **Análise Exploratória de Dados (AED):**
    *   Estatísticas descritivas foram calculadas para variáveis numéricas (`tenure`, `Charges.Monthly`, `Charges.Total`).
    *   Visualizações (Histogramas e Box Plots) foram geradas para entender a distribuição das variáveis numéricas e sua relação com o *churn*.
    *   Gráficos de contagem foram criados para analisar a relação entre variáveis categóricas e o *churn*, identificando perfis de clientes com maior ou menor taxa de evasão.
    *   A proporção da classe de *churn* foi calculada para avaliar o desequilíbrio de classes no conjunto de dados.
4.  **Tratamento de Desequilíbrio de Classes e Valores Ausentes:**
    *   Valores ausentes (`NaN`) nas colunas numéricas do conjunto de treinamento foram imputados utilizando a média.
    *   A técnica SMOTE (Synthetic Minority Over-sampling Technique) foi aplicada ao conjunto de treinamento para aumentar o número de amostras da classe minoritária (*churn*), equilibrando o conjunto de dados para o treinamento do modelo.
    *   Os mesmos passos de imputação foram aplicados ao conjunto de teste para garantir consistência.
5.  **Modelagem Preditiva:**
    *   Um modelo de Regressão Logística foi treinado no conjunto de treinamento balanceado.
    *   O modelo foi avaliado no conjunto de teste usando métricas como Matriz de Confusão, Relatório de Classificação (Precisão, Recall, F1-score) e Acurácia.
    *   As probabilidades de *churn* foram calculadas para os clientes no conjunto de teste, permitindo identificar os clientes com maior risco de evasão.
    *   Os coeficientes do modelo de Regressão Logística foram analisados para identificar as variáveis mais influentes na previsão de *churn*.

## Resultados e Conclusões

### Análise Exploratória

*   Foi confirmado um desequilíbrio significativo no conjunto de dados, com a maioria dos clientes não apresentando *churn*.
*   Clientes com menor tempo de serviço (`tenure`) e maiores cobranças mensais (`Charges.Monthly`) mostraram maior propensão ao *churn*.
*   Características como ter serviço de internet Fibra Óptica, não possuir serviços adicionais de segurança online/suporte técnico, ter contrato Mês a Mês e utilizar Cheque Eletrônico como método de pagamento foram fortemente associadas a taxas mais altas de *churn*.
*   Clientes com contratos de longo prazo (Um Ano ou Dois Anos) e aqueles que utilizam métodos de pagamento automáticos (transferência bancária ou cartão de crédito) apresentaram menor probabilidade de *churn*.

### Modelagem Preditiva (Regressão Logística)

*   O modelo de Regressão Logística treinado nos dados balanceados obteve uma acurácia geral de aproximadamente 76.25%.
*   A avaliação detalhada mostrou que o modelo performa melhor na previsão da classe majoritária (não *churn*). A precisão e recall para a classe *churn* (classe minoritária) foram menores, indicando que há espaço para melhoria na identificação dos clientes que realmente irão evadir.
*   A análise dos coeficientes do modelo confirmou as variáveis identificadas na AED como as mais influentes, destacando o serviço de internet (especialmente Fibra Óptica), serviços adicionais online, tipo de contrato e método de pagamento como preditores chave do *churn*.
*   Foi possível identificar os clientes com maior probabilidade de *churn* com base nas previsões do modelo.

### Perfil de Cliente de Baixo Risco de Evasão

Com base nas variáveis mais influentes, o perfil de cliente ideal (com menor risco de evasão) tende a ser:

*   Clientes de longa data (`tenure` alto).
*   Clientes com contratos de longo prazo (Um Ano ou Dois Anos).
*   Clientes que utilizam métodos de pagamento automáticos (transferência bancária ou cartão de crédito).
*   Clientes que assinam serviços adicionais de segurança online, backup online, proteção de dispositivo e suporte técnico.
*   Clientes que não utilizam o serviço de internet Fibra Óptica ou estão satisfeitos com ele.
*   Clientes não idosos, com parceiro e dependentes.

## Próximos Passos Sugeridos

*   **Investigação Aprofundada:** Realizar análises mais detalhadas sobre os motivos por trás da alta taxa de *churn* em segmentos específicos (ex: usuários de Fibra Óptica, método de pagamento Cheque Eletrônico).
*   **Estratégias de Retenção:** Desenvolver e implementar campanhas de retenção segmentadas com base nos perfis de risco identificados e nas variáveis mais influentes.
*   **Melhoria do Modelo:** Explorar outros algoritmos de machine learning (como Random Forest, Gradient Boosting, SVM), otimizar hiperparâmetros do modelo atual, ou realizar engenharia de features mais avançada para tentar melhorar a performance na previsão da classe *churn*.
*   **Validação Cruzada:** Implementar validação cruzada para obter uma estimativa mais robusta e confiável do desempenho do modelo.
