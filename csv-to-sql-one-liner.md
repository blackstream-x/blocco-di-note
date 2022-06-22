# CSV to SQL(ite3) one-liner

_Adapted from <https://til.simonwillison.net/sqlite/one-line-csv-operations>,
extracting the 7z file to stdout and feeding it to SQLite through a pipeline_

Downloaded <https://github.com/multiprocessio/dsq/blob/0.20.1/testdata/taxi.csv.7z> to `~/Downloads`

```
$ time ( 7z e -so Downloads/taxi.csv.7z | sqlite3 :memory: -cmd '.mode csv' -cmd '.import "| cat -" taxi' -cmd '.mode markdown' 'SELECT passenger_count, COUNT(*), AVG(total_amount) FROM taxi GROUP BY passenger_count' )
| passenger_count | COUNT(*) | AVG(total_amount) |
|-----------------|----------|-------------------|
|                 | 128020   | 32.2371511482553  |
| 0               | 42228    | 17.0214016766151  |
| 1               | 1533197  | 17.6418833067999  |
| 2               | 286461   | 18.0975870711456  |
| 3               | 72852    | 17.9153958710923  |
| 4               | 25510    | 18.452774990196   |
| 5               | 50291    | 17.2709248175672  |
| 6               | 32623    | 17.6002964166367  |
| 7               | 2        | 87.17             |
| 8               | 2        | 95.705            |
| 9               | 1        | 113.6             |

real	0m6,796s
user	0m7,044s
sys	0m0,434s
```

