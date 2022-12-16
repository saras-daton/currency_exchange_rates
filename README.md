# Currency Exchange Rates Modelling
This dbt package models the Exchange Rates data coming from [Daton](https://sarasanalytics.com/daton/). [Daton](https://sarasanalytics.com/daton/) is the Unified Data Platform for Global Commerce with 100+ pre-built connectors and data sets designed for accelerating the eCommerce data and analytics journey by [Saras Analytics](https://sarasanalytics.com).

This package would be performing the following funtions:

- Consolidation - Different marketplaces & different brands would have similar tables. Helps in consolidating all the tables into one final stage table 
- Deduplication - Based on primary keys , the tables are deduplicated and the latest records are only loaded into the stage models
- Incremental Load - Models are designed to include incremental load which when scheduled would update the tables regularly

#### Prerequisite 
Daton Connector for Currency Exchange Data - Exchange Rates

# Installation & Configuration

## Installation Instructions

If you haven't already, you will need to create a packages.yml file in your project. Include this in your `packages.yml` file

```yaml
packages:
  - package: saras-daton/currency_exchange_rates
    version: 1.0.0
```

# Configuration 

## Required Variables

This package assumes that you have an existing dbt project with a BigQuery profile connected & tested. Source data is located using the following variables which must be set in your `dbt_project.yml` file.

```yaml
vars:
    raw_projectid: "your_gcp_project"
    raw_dataset: "your_amazon_ads_dataset"
```

## Schema Change

We will create the models under the schema (<target_schema>_stg_amazon). In case, you would like the models to be written to the target schema or a different custom schema, please add the following in the dbt_project.yml file.

```yml
models:
  currency_exchange_rates:
    +schema: custom_schema_name # leave blank for just the target_schema
```

## Optional Variables

Package offers different configurations which must be set in your `dbt_project.yml` file under the above variables. These variables can be marked as True/False based on your requirements. Details about the variables are given below.

```yaml
vars:
    currency_conversion_flag: True
```

### Currency Conversion 

We would need to set the currency conversion flag to enable the model creation. By marking it as False, it generates an empty model.

## Scheduling the Package for refresh

The exchange rate tables that are being generated as part of this package are enabled for incremental refresh and can be scheduled by creating a job in Production Environment. During 'dbt Build', the models get refreshed.

## Models

This package contains the Exchange Rates model coming from the Exchange Rates Daton connector. Please follow this to get more details about [models](https://docs.google.com/spreadsheets/d/1OaJnVpBrPZaBusJXBHrT8dhnD2zctWMmSRN__WLQsl0/edit?usp=sharing). The primary outputs of this package are described below.

| **Category**                 | **Model**  | **Description** |
| ------------------------- | ---------------| ----------------------- |
|Currency Exchange | [ExchangeRates](models/Currency%20Exchange%20Rates/ExchangeRates.sql)  | A list of portfolios associated with the account |


## Resources:
- Have questions, feedback, or need [help](https://calendly.com/priyanka-vankadaru/30min)? Schedule a call with our data experts or email us at info@sarasanalytics.com.
- Learn more about Daton [here](https://sarasanalytics.com/daton/).
- Refer [this](https://youtu.be/6zDTbM6OUcs) to know more about how to create a dbt account & connect to Bigquery
