---
title: "Hive"
date: 2021-11-22
---

``` sql
CREATE TABLE midasbuy_tob_t_tob_remitpay_records_v1(
    statis_date STRING,
    freceiptno STRING,
    fvirtualacct STRING,
    fcurrencyrate STRING,
    fbiztype STRING,
    fmdmname STRING,
    fbankaccountnum STRING,
    fmdmid STRING,
    fopenid STRING,
    famount STRING,
    faccounttype STRING,
    fpaidname STRING,
    fcurrencycode STRING,
    freceiptdate STRING,
    fmerchantid STRING,
    frequesttime STRING
)
PARTITION BY RANGE( statis_date )
(
    PARTITION p_20211121 VALUES LESS THAN ( '20211122' ),
    PARTITION default
)
STORED AS ORCFILE COMPRESS;
```
