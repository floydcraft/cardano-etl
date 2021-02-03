
[![Cardano Project Catalyst Fund3 - Discord](https://img.shields.io/badge/discord-join%20chat-blue.svg)](https://discord.gg/QRBcuvbQ)

## Overview
[Cardano ETL](https://github.com/floydcraft/cardano-etl) is a proposal for [Project Catalyst Fund3](https://cardano.ideascale.com/a/dtd/Cardano-ETL-Public-BigQuery-Data/334530-48088).

## Problem
Collection and democratization of data in the Cardano ecosystem will be critical for feeding new ideas and measuring critical project KPIs.

## Solution
Cardano ETL will support transformation of Cardano blockchain data into convenient formats like CSV, JSON Newline, GCP PubSub, and relational databases.

Initially we will support exporting CSV, JSON Newline Delimited, GCP PubSub. We will also operate a passive stakepool that streams in realtime to a public BigQuery dataset for everyone to use using PubSub/Dataflow pipeline/BigQuery.

We will expand into additional convenient formats in later phases and milestones likely based on the success of this Fund3 effort (both technology and vlog lessons/amas/deep dives).

If you want instant Cardano data and you rather NOT export the blockchain yourself using `cardanoetl`; please checkout the quickstart below for our realtime public BigQuery data!

## Target Impact
- Meaningful step towards the democratization of Cardano Data in a scalable and accessible way with both batch and streaming support.
- Realtime Cardano public BigQuery data operated with validation.
- Useful Cardano insights derived from raw, summerized, and aggregate data by developers, data scientists/engineers, and product owners that minimized/eliminates technical blockers for access to those insights.

## Auditability
- Opt-out/optional reporting of anonymous usage statistics of Cardano ETL features also as public BigQuery data.
- Shared learnings of Cardano / Haskell / Catalyst / Proposal with - community via Youtube / Twitter
- Open Source [cardano-etl](https://github.com/floydcraft/cardano-etl)
- Weekly public goals with demos (vlog, notebooks, ...)
- Ideally open source contributions from the community
- Potentially community showcases of their insights and how they derived them
- Some metrics that will have a dashboard once development is complete. Until then, can be reported when applicable.
  - BigQuery Queries / day
  - BigQuery TB scanned / day ($5 USD per TB scanned after first TB)
  - Unique users / day
  - Infrastructure Spend / day
  - Github Insights / day (Bugs, Resolved, Comments, ...)
- Github Issues / PR's for public project tracking

## Feasibility
### Approach
Well great news! IOHK already has a model for how to sync the blockchain to a SQL database https://github.com/input-output-hk/cardano-db-sync .

Initially I’ll work quickly to see if Haskell only solution works much like the cardano-db-sync repo currently uses. I suspect that will actually not scale to many target db/formats and allow for a clean/useful implementation, but I need to follow up on this as my first action item.

So, in the case I can't use pure Haskell I'll end up using the serialization lib to load the data from disk into python which will allow the Cardano ETL project to target just about any potential target (BigQuery, Athena, Json, …). This is more or less the Ethereum ETL approach https://github.com/blockchain-etl/ethereum-etl . This might require work to make the serialization lib available in python (TODO).

Worst case I’ll end up needing to improve/add features to the serialization lib to allow for the second approach.

All of this will be open source and I welcome contributions / feedback along the way.

One note on the Cardano ETL CLI. The idea is that it supports exporting to all likely formats/streams, but it could be that a limited set is supported via a Haskell CLI and a full set is supported via a python CLI (like PubSub streaming).

### Applicable Skills
- 4 years of in depth BigData experience with both architecture and implementations on AWS/GCP
- Combined 13 years of professional engineering for simulations and mobile games (C++/C#/python/OpenGL/Unity3D/networking)
- Over a year of in depth commitment to understand Cardano and it's many tentacles through blogs, white papers, just about every Charles video, and Fund2 as examples.
- Setup and operated a jormungandr stakepool (CHBFI) for a few months.
- Modified the stakepool to expose additional API's for potential stakepool discovery service/website. It was a dead end with the approach I took, but it made for a cool frontend for my pool at the time that nobody cared about =P
- Basically no Haskell experience (WIP), but after stakepool setup is up and working and I'll figure out the iterative dev workflows.

### Estimates
- 3-4 months of Cardano ETL Development (20K USD)
  - Third Party Subscriptions / Tools
  - ~200 Hours over 3 months to get to alpha / beta (open source github)
  - ~200 Hours over following 6 months for production + monitoring and operations (GCP kubernetes)
- 6 months of GCP operations usage costs (8K USD)
  - ~40% ETL Compute/Networking/GKE Managed Kubernetes/Stackdriver/...
  - ~60% BigQuery Storage and Queries (w/ buffer for 50TB scanned per month)
- Cardano ETL BigQuery Data use case marketing (2K USD)
  - specific use case YouTube playlists/notebooks/sessions for helping developers, data scientists/engineers, and product owners understand how to use the Cardano data to evaluate both opportunities and their project operations.
  - attempt to showcase Google GCP with the technical approach to this project for potential Google developer marketing opportunities
  - some materials/assets to support the marketing efforts

### Resourcing
- Nights and weekends for me (reasonable for myself because I like to develop with intent/purpose).
- Ideally find a partner with the right skillsets and alignment of core values to contribute / contract.
- Use open source model for less direct potential contributions (bugs, target exporters) once the framework is in place.

## Future Funding (for both operations and developments)
- Will setup a Cardano ETL Stakepool that purpose is contributing it's proceeds to the funding of this project
- Potentially/likely to make proposals when it makes sense through Catalyst
- Would love to share/grow my data driven knowledge and experiences with the community if it turns out to be interesting and reinvest that into this foundation.
- Curious if anyone has ideas that I should be considering (dApps) that I'm not that would lend itself to a healthy operations / investments in the democratization of Cardano data.

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
