# Amazon Sales Analytics: Estratégia e Logística de E-commerce

## 1. Contexto de Negócio
Este projeto baseia-se em um conjunto de dados reais de vendas da **Amazon Índia**. O cenário simulado é o de um Analista de BI que precisa fornecer respostas rápidas para a diretoria sobre o desempenho das vendas, eficiência logística e perfil do consumidor.

**Principais Desafios:**
* Identificar padrões de vendas ao longo do tempo.
* Analisar a eficiência dos diferentes métodos de entrega (*Fulfilment*).
* Monitorar a saúde financeira através da taxa de cancelamento e ticket médio.

---

## 2. Arquitetura da Solução e ETL
A fase de tratamento de dados foi a mais crítica, garantindo a integridade da análise.

* **Fonte de Dados:** Arquivo CSV contendo +120 mil linhas de transações.
* **Limpeza (Power Query):**
    * **Normalização de Datas:** Tratamento de erro de localidade (MM-DD-AA) para o padrão de data reconhecido pelo Power BI.
    * **Enriquecimento de Dados:** Criação de colunas condicionais para simplificar o Status do pedido (Sucesso vs. Cancelado).
    * **Padronização Geográfica:** Limpeza de nomes de cidades e estados para correta plotagem no mapa.
* **Modelagem de Dados:**
    * Aplicação do **Star Schema** (Esquema Estrela).
    * Criação de uma tabela `dCalendário` via DAX para análises temporais precisas (YoY, MoM).

---

## 3. Dashboards e KPIs
O dashboard foi dividido em três pilares principais:

### A. Performance de Vendas
* **Faturamento Total e Ticket Médio:** Visão rápida da receita bruta e do valor gasto por pedido.
* **Sazonalidade:** Gráfico de linhas identificando dias de pico (prováveis períodos promocionais).

### B. Eficiência Operacional (Logística)
* **Fulfilment Analysis:** Comparação de vendas enviadas pela própria Amazon vs. Vendedores terceiros (Merchants).
* **Distribuição Geográfica:** Mapa de calor identificando os estados de Maharashtra, Karnataka e Telangana como os maiores polos consumidores.

### C. Gestão de Produto
* **Pareto de Categorias:** Identificação de que as categorias "Set" e "Kurta" representam a maior parte do volume financeiro.
* **Análise de Grade (Size):** Monitoramento de quais tamanhos têm maior saída para otimização de estoque.

---

## 4. Medidas DAX Utilizadas
Para este projeto, foram desenvolvidas medidas personalizadas para métricas de negócio. Abaixo, alguns exemplos:

```dax
// Cálculo de Ticket Médio
Ticket Médio = 
DIVIDE(
    SUM('Amazon Sales'[Amount]), 
    COUNTROWS('Amazon Sales'), 
    0
)

// Taxa de Cancelamento
% Cancelados = 
VAR TotalCancelados = CALCULATE(COUNTROWS('Amazon Sales'), 'Amazon Sales'[Status] = "Cancelled")
VAR TotalPedidos = COUNTROWS('Amazon Sales')
RETURN 
    DIVIDE(TotalCancelados, TotalPedidos, 0)

```
---
Contato

Caroline Lopes Martins - www.linkedin.com/in/caroline-lopes-martins-2911b734b 
