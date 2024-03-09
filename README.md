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
## Convert all files in the current folder
```
for i in *.jpg; do convert "$i" -gamma 2.7 "${i%.*}_gamma.jpg"; done
```
## Reminder in e-mail
```
#!/bin/bash

# Set the renewal date
renewal_date="09"

# Get the current date
current_date=$(date +"%d")

# Check if the renewal date is approaching
if [ "$current_date" == "$renewal_date" ]; then
    # If the renewal date is today, send a notification email
    echo "Please renew your VPS today!" | mail -s "VPS Renewal Reminder" info@mail.com
fi
```
