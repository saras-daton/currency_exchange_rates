# Currency Exchange Rates Modelling
This DBT package models the Exchange Rates data coming from [Daton](https://sarasanalytics.com/daton/). [Daton](https://sarasanalytics.com/daton/) is the Unified Data Platform for Global Commerce with 100+ pre-built connectors and data sets designed for accelerating the eCommerce data and analytics journey by [Saras Analytics](https://sarasanalytics.com).

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
  - package: daton/currency_exchange_rates
    version: 0.1.0
```

# Configuration 

## Required Variables

This package assumes that you have an existing DBT project with a BigQuery profile connected & tested. Source data is located using the following variables which must be set in your `dbt_project.yml` file.

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
    currency_conversion_flag: False
```

### Currency Conversion 

To enable currency conversion, which produces two columns - conversion_rate, conversion_currency based on the data from the Exchange Rates Connector from Daton.  please mark the currency conversion flag as True. By default, it is False.

## Scheduling the Package for refresh

The ad tables that are being generated as part of this package are enabled for incremental refresh and can be scheduled by creating the job in Production Environment by giving the below command.

```
dbt run --select currency_exchange_rates
```

## Models

This package contains models from the Amazon API which includes Sponsored Brands, Products, Display. The primary outputs of this package are described below.

| **Category**                 | **Model**  | **Description** |
| ------------------------- | ---------------| ----------------------- |
|Currency Exchange | [ExchangeRates](models/Currency%20Exchange%20Rates/ExchangeRates.sql)  | A list of portfolios associated with the account |


## Resources:
- Have questions, feedback, or need [help](https://calendly.com/priyanka-vankadaru/30min)? Schedule a call with our data experts or email us at info@sarasanalytics.com.
- Learn more about Daton [here](https://sarasanalytics.com/daton/).
- Refer [this](https://youtu.be/6zDTbM6OUcs) to know more about how to create a DBT account & connect to Bigquery
