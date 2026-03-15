# Data Warehouse de Vendas

Projeto de Engenharia de Dados focado na construção de um **Data Warehouse de vendas** utilizando **Apache Spark**, **SQL** e **Delta Lake** no **Databricks**.

O objetivo é transformar dados transacionais em um modelo analítico no formato **Star Schema**, permitindo análises e métricas de negócio relevantes.

---

## Arquitetura do Projeto

Pipeline clássico de camadas:

- **Bronze** → dados brutos (raw)
- **Silver** → dados limpos, tratados e enriquecidos
- **Gold** → modelo dimensional (fato + dimensões)

Etapas implementadas:

- Ingestão e camada Bronze
- Limpeza e transformação Silver
- Modelagem Star Schema na camada Gold
- Consultas analíticas e métricas de negócio

---

## Dataset

**Sales Forecasting Dataset** (Kaggle)

Principais entidades:

- Pedidos
- Clientes
- Produtos
- Vendas
- Datas

---

## Tecnologias Utilizadas

- Apache Spark (PySpark)
- Databricks
- Spark SQL
- Delta Lake
- Python

---

## Modelagem Dimensional – Star Schema

**Tabela Fato**  
- `fact_sales`

**Tabelas Dimensão**  
- `dim_customer`  
- `dim_product`  
- `dim_time`

## Modelo Estrela (Star Schema)

       dim_customer
            |
            |

dim_product --- fact_sales --- dim_time


---

## Principais Consultas Analíticas

### 1. Receita Mensal

```sql
SELECT 
    YEAR(order_date) AS ano,
    MONTH(order_date) AS mes,
    ROUND(SUM(sales), 2) AS receita_total
FROM fact_sales
GROUP BY YEAR(order_date), MONTH(order_date)
ORDER BY ano, mes;

SELECT 
    ROUND(AVG(sales), 2) AS ticket_medio
FROM fact_sales;
```
### 2. Ticket Médio Geral
   
```sql
SELECT 
    p.product_name,
    ROUND(SUM(f.sales), 2) AS receita_total
FROM fact_sales f
INNER JOIN dim_product p ON f.product_id = p.product_id
GROUP BY p.product_name
ORDER BY receita_total DESC
LIMIT 10;
```
### 3. Top 10 Produtos por Receita
```sql
SELECT 
    p.product_name,
    ROUND(SUM(f.sales), 2) AS receita_total
FROM fact_sales f
INNER JOIN dim_product p ON f.product_id = p.product_id
GROUP BY p.product_name
ORDER BY receita_total DESC
LIMIT 10;
```
### Exemplos de Insights:

Evolução da receita ao longo do tempo
Identificação dos produtos mais rentáveis
Análise de sazonalidade
Crescimento mês a mês
Ticket médio por período

---

## Objetivo do projeto:

- Demonstrar conhecimentos em:

- Engenharia de Dados

- Modelagem dimensional

- SQL analítico

- Apache Spark

- Data Warehouse

---

## Próximos Passos / Melhorias

- Dashboards interativos (Databricks SQL / Power BI)
- Pipeline automatizado Bronze → Silver → Gold
- Ingestão incremental
- Orquestração (Databricks Workflows / Airflow)
- Testes de qualidade de dados
- Monitoramento e alertas

