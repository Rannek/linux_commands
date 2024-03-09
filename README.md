## Download one folder a webserver

``` wget -np -r [link] ```

## Download links in a timeframe

```
#!/bin/bash
startdate=2022-07-11
enddate=2022-08-16

d="$startdate"

while [ "$d" != "$enddate" ]; do 
  yt-dlp https://soundcloud.com/balazsek/sets/$d-adas
  d=$(date -I -d "$d + 1 day")
done
```
## Incremental tar backup
```
tar -cvg snapshot-file -f backup.tar /
tar -cvg snapshot-file -f backup_1.tar /
```
