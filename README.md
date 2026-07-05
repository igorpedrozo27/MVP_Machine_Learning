# Predição de Refugo Industrial na Onduladeira (MVP)

Este repositório contém o Produto Mínimo Viável (MVP) desenvolvido como parte da Pós-Graduação. O projeto aplica técnicas de Engenharia e Análise de Dados além de Machine Learning para prever o volume diário de perdas (refugo) em uma máquina onduladeira de papelão, atuando como ferramenta de apoio à decisão para o Planeamento e Controlo de Produção (PCP).

## Contexto do Problema

O processo de fabricação de papelão ondulado é físico, térmico e altamente sensível a interrupções. Atualmente, a fábrica opera com perdas significativas de matéria-prima (refugo) resultantes de setups excessivos e instabilidade da máquina. Como a fábrica está passando por um processo de digitalização, os dados históricos são limitados (Small Data), mas de alta confiança.

O objetivo deste projeto é substituir a intuição por estatística, respondendo à pergunta: Com base no plano de produção de hoje (volume, setups esperados e momento do mês), quantos quilos de refugo a máquina irá gerar amanhã?

## Objetivos e Critérios de Sucesso

Objetivo Principal: Prever o volume diário de refugo da onduladeira (em Kg) utilizando algoritmos de Regressão.

Métrica de Avaliação: Erro Médio Absoluto (MAE), escolhido por refletir o erro real em quilogramas, facilitando a interpretação pela controladoria de custos.

Critério de Sucesso: Superar a performance de um modelo de base (Regressão Linear Múltipla) utilizando algoritmos mais complexos que consigam lidar com a não-linearidade do chão de fábrica, mantendo o modelo interpretável e imune a overfitting.

## O Conjunto de Dados (Dataset)

A Base Analítica (Analytical Base Table - ABT) foi construída através do cruzamento de múltiplas tabelas transacionais extraídas do sistema de apontamento e base de dados da fábrica (Produção, Paradas, Tempo de Turno e Calendário).

Granularidade: Diária.

Volume: 128 dias úteis de operação (após limpeza e consolidação).

Atributos Principais: Volume_Kg (escala produtiva), Qtd_Total_Paradas (instabilidade), Qtd_Setups_Rodados (fragmentação de pedidos), Semana_Mes (sazonalidade gerencial).

## Metodologia Aplicada

Engenharia de Dados (ETL): Padronização temporal, pivotamento de tabelas de paradas e criação de variáveis sintéticas de eficiência operacional.

Análise Exploratória (EDA): Identificação de outliers operacionais (preservados, pois representam quebras reais da máquina) e análise bivariada para compreender a relação de escala e frequência de falhas.

Seleção de Variáveis: Remoção de variáveis com multicolinearidade (ex: PRODUZIDO_Onduladeira vs Volume_Kg) e afunilamento para os vetores de maior impacto no negócio.

Modelação (Validação Cruzada 5-Fold):

Baseline: Regressão Linear Múltipla.

Challengers: Random Forest, XGBoost e LightGBM.

Otimização (Tuning): Aplicação de GridSearchCV para encontrar a profundidade ideal da árvore e evitar tanto o Underfitting quanto o Overfitting característico em Small Data.

## Principais Descobertas (Insights de Negócio)

O Custo do "Liga-Desliga": A análise revelou que a frequência de paradas afeta muito mais o refugo do que o tempo total em que a máquina esteve parada. Interrupções curtas e sucessivas quebram a estabilidade térmica da onduladeira.

A Síndrome do "Fim de Mês": Variáveis categóricas demonstraram que a pressão por metas de faturação nas últimas semanas do mês obriga o PCP a fragmentar o planeamento (mais setups), aumentando estatisticamente e drasticamente o desperdício médio diário.

O Modelo Campeão: O Random Forest Otimizado (max_depth=5) provou ser o algoritmo ideal para este cenário. Modelos mais profundos (XGBoost) sofreram forte overfitting devido ao baixo volume de linhas (128 dias), enquanto a Regressão Linear apresentou ligeiro underfitting por não capturar as anomalias exponenciais de dias muito instáveis.

## Tecnologias e Bibliotecas

Linguagem: Python 3

Manipulação de Dados: Pandas, NumPy

Visualização: Matplotlib, Seaborn

Machine Learning: Scikit-Learn, XGBoost, LightGBM
