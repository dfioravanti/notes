# Athena

## Overview

Serverless interactive querying service. You pay only the data consumed (and S3 storage). It applies a *schema on read* conversion. The original data is never changed but athena parse the data via a user defined schema and it appears relational on read. The output can be send to other services.

    S3 -> Schema -> SQL

