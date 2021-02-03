
[![Cardano Project Catalyst Fund3 - Discord](https://img.shields.io/badge/discord-join%20chat-blue.svg)](https://discord.gg/QRBcuvbQ)

## Overview

[Cardano ETL](https://github.com/floydcraft/cardano-etl) is a proposal for [Project Catalyst Fund3](https://cardano.ideascale.com/a/dtd/Cardano-ETL-Public-BigQuery-Data/334530-48088) and will support transformation of Cardano blockchain data into convenient formats like CSV, JSON Newline, GCP PubSub, and relational databases.

Initially we will support exporting CSV, JSON Newline Delimited, GCP PubSub. We will also operate a passive stakepool that streams in realtime to a public BigQuery dataset for everyone to use using PubSub/Dataflow pipeline/BigQuery.

We will expand into additional convenient formats in later phases and milestones likely based on the success of this Fund3 effort (both technology and vlog lessons/amas/deep dives).

If you want instant Cardano data and you rather NOT export the blockchain yourself using `cardanoetl`; please checkout the quickstart below for our realtime public BigQuery data!

## Quickstarts
### Public BigQuery data: GCP BigQuery Console
> NOTE: this section is works atm, but uses a copy of the public ethereum BigQuery dataset. The intent to to provide a demo of how it'll work, those the schema / data will be Cardano once the proposal is complete.

1. (OPTIONAL) [Sign in, agree to the Google TOS, and create a project](https://console.cloud.google.com/projectcreate) on Google Cloud Platform (GCP) using a Google  account. e.g., `myproject-xyz`

2. [Navigate to BigQuery Console](https://console.cloud.google.com/projectcreate) for your GCP project.

3. Copy, paste, and run the following SQL in a SQL IDE tab

```postgresql
SELECT DATE(timestamp) as tx_day,
        AVG(gas_used) as avg_gas_used,
        SUM(gas_used) as total_gas_used,
        AVG(gas_limit) as avg_gas_limit,
        SUM(gas_limit) as total_gas_limit,
        AVG(transaction_count) as avg_transaction_count,
        SUM(transaction_count) as total_transaction_count
FROM `crypto-data-prod.crypto_cardano.blocks`
WHERE DATE(timestamp) >= "2020-01-01"
GROUP BY 1
ORDER BY 1
LIMIT 1000
```

4. Congratulations! You should now see a `Query results` section with a table and all kind's of interesting capabilities =)

### Public BigQuery data: Kaggle Notebook
> NOTE: this section is works atm, but uses a copy of the public ethereum BigQuery dataset. The intent to to provide a demo of how it'll work, those the schema / data will be Cardano once the proposal is complete.

1. [Goto Kaggle Notebook - Cardano Simple Example](https://www.kaggle.com/chbourkefloydiv/cardano-bigquery-simple)

2. Review the Kaggle/jupyter notebook example in python!

3. (OPTIONAL) [Sign in](https://www.kaggle.com/account/login?phase=startSignInTab&returnUrl=%2Fchbourkefloydiv%2Fcardano-bigquery-simple) or [Register](https://www.kaggle.com/account/login?phase=startRegisterTab&returnUrl=%2Fchbourkefloydiv%2Fcardano-bigquery-simple) for Kaggle.

4. (OPTIONAL) Select [Copy and Edit](https://www.kaggle.com/kernels/fork-version/52817044)

5. (OPTIONAL) Select Run All or Run each cell from top to bottom to execute the python code. Read and follow the notes. Make edits!

6. Congratulations! You should now see the `output per cell with plots`!

### Cardano ETL CLI
> NOTE: this section is hypothetical atm (modeled after ethereum-etl), but is the general approach planned.

Install Cardano ETL (python3):

```bash
pip3 install cardano-etl
```

Export blocks and transactions:

```bash
> cardanoetl export_blocks --start-block-hash 1a3be38bcbb7911969283716ad7aa550250226b76a61fc51cc9a9a35d9276d81
--output blocks.json --output-format json-newline-delimited \
--provider-uri X.X.X.X
```

```bash
> cardanoetl export_transactions --start-block-hash 1a3be38bcbb7911969283716ad7aa550250226b76a61fc51cc9a9a35d9276d81
--output transactions.json --output-format json-newline-delimited \
--provider-uri X.X.X.X
```

## Cardano ETL: Public BigQuery Data
> Project Catalyst Fund3 Proposal
### Plan



Overview / Fund 3

3-4 Months Development Part Time (Nights and Weekends)
Target 3 Months of Alpha / Beta
6 Months of Operations
Vlog my experience digging into Cardano / Haskell / Catalyst / This Proposal
Target weekly Youtube AMA’s if the community shows interest
Big Data?
BigQuery?
ETL?





## YouTube Playlist: [Cardano ETL](https://www.youtube.com/watch?v=QeFCzwNBR5U&list=PLy-_xx3OXqCkGookx2F3ob2_fuPVx2uqW)
### Cardano ETL: [Public BigQuery Data](https://www.youtube.com/watch?v=QeFCzwNBR5U)

Collection and democratization of data in the Cardano ecosystem will be critical for feeding new ideas and measuring critical project KPIs

- Introduction
- Project Catalyst Proposal
- Proposal Target Impact
- Proposal Audit-ability
- Proposal Feasibility

[![Test](https://img.youtube.com/vi/QeFCzwNBR5U/0.jpg)](https://www.youtube.com/watch?v=QeFCzwNBR5U)

### Cardano ETL: [Public BigQuery Examples](https://www.youtube.com/watch?v=0LtND_PDfQU)

Cardano ETL: Public BigQuery Examples for Cardano Catalyst Proposal Fund 3

- [BigQuery](https://cloud.google.com/bigquery)
- [Kaggle](https://www.kaggle.com/chbourkefloydiv/cardano-bigquery-simple)
- [Data Studio](https://datastudio.google.com/reporting/02079fd9-b455-440d-8643-be2862f01931)
- [Google Sheets](https://support.google.com/docs/answer/9702507?hl=en)

[![Test](https://img.youtube.com/vi/0LtND_PDfQU/0.jpg)](https://www.youtube.com/watch?v=0LtND_PDfQU)
