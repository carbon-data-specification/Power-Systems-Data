# Data interfacing

### Timestamp
***NOTE:*** Timestamps should only be provided in UTC. Information to convert timestamp into local prevailing time will be a static property of the spatial "node" with which each data source is associated.

|Data column                             |Unit              |Type    |Definition                                                                |Notes                                                                                                                       |
|----------------------------------------|------------------|--------|--------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|
|datetime_utc                                |ISO format (UTC)  |datetime|ex: 2018-31-01T00:00:00+00:00                                             |The datetime represents the start of hour e.g. 16:00 represents data from 16:00-16:59