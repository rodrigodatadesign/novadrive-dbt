# novadrive-dbt

dbt project transforming raw sales data (vehicles, dealerships, salespeople, customers) into analytics-ready models on Snowflake. Part of a full data pipeline built with Airflow (orchestration), Snowflake (warehouse, RSA key-pair authentication), and Power BI (dashboard) — this repo covers the transformation layer.

## Pipeline

1. ✅ Raw sales data sourced from PostgreSQL (vehicles, dealerships, salespeople, customers)
2. ✅ Loaded and staged into Snowflake (`staging`)
3. ✅ Cross-entity relationships resolved (`intermediate`)
4. ✅ Analytical marts consumed by the dashboard (`marts`)
5. ✅ Orchestrated via Apache Airflow, self-hosted (Docker Swarm + Traefik)

## Tech Stack

- **Transformation:** dbt
- **Warehouse:** Snowflake (RSA key-pair authentication)
- **Orchestration:** Apache Airflow, self-hosted on a Docker Swarm VPS

## Data Model

**staging** — per-source normalization of raw sales data sourced from PostgreSQL

**models (fact & dimensions)**
```
fct_vendas          # VENDA_ID, VEICULO_ID, CONCESSIONARIA_ID, VENDEDOR_ID,
                     # CLIENTE_ID, VALOR_VENDA, DATA_VENDA
dim_veiculo          # vehicle model, suggested price
dim_vendedor         # salesperson
dim_concessionaria   # dealership, city, state
dim_cliente          # customer
```

**marts** — analytical tables consumed by the Power BI dashboard
```
analise_vendas_veiculo         # sales aggregated by vehicle model
analise_vendas_vendedor        # sales aggregated by salesperson
analise_vendas_concessionaria  # sales aggregated by dealership / city / state
analise_vendas_temporal        # sales aggregated by month
```

## Setup

```bash
# 1. Clone the repository
git clone https://github.com/rodrigodatadesign/novadrive-dbt.git
cd novadrive-dbt

# 2. Install dbt and dependencies
pip install dbt-snowflake

# 3. Configure your Snowflake connection profile
# (profiles.yml — RSA key-pair authentication)

# 4. Run the models
dbt run
dbt test
```

## Project Structure

```
novadrive-dbt/
├── models/
│   ├── staging/       # sources: PostgreSQL (loaded into Snowflake)
│   ├── intermediate/
│   └── marts/         # consumed by Power BI + orchestrated by Airflow
├── seeds/
├── snapshots/
├── macros/
├── analyses/
├── tests/
├── dbt_project.yml
└── README.md
```
