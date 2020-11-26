# Redshift

## Overview

Petabyte scale data warehouse, it is a column based database. It is designed to aggregate and analyse data. Pay as you use. It is not serverless. Features:

* Directly query S3 with Redshift spectrum
* Directly query other DB with Redshift federate query
* Integrates with quicksight

The architecture runs on its own cluster and
* available in one AZ
* You interact with one leader node
* compute node run the jobs
* Redshift enhanced VPC routing gives access to the internal VPC of redshift