# Working with Dates and Times in Python

```
from datetime import date
date_obj = date(1992, 8, 27)

# Which day of the week is the date?
print(date_obj.weekday()
```
- ISO: Date: 1950-08-31  - Formatt = "%Y-%m-%dT%H:%M:%S"
- US:  Date: 08/31/1950
- IND: Date: 31/08/1950

|     |         |     |
|-----|-----------|----|
|%Y|4 |digit year (0000-9999) |
|%m	|2| digit month (1-12)|
|%d	|2|digit day (1-31)|
|%H	|2| digit hour (0-23)|
|%M	|2| digit minute (0-59)|
|%S	|2| digit second (0-59)|
