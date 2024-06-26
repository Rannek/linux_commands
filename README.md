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

## Create swap

```
sudo fallocate -l 32G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
/swapfile   none    swap    sw    0   0
free -h
```

## Kernel gen script
```
make clean
make -j8
make modules_install
cp -v /usr/src/linux-6.6.10-zen/arch/x86/boot/bzImage /boot/kernel-6.6.10-zen
genkernel initramfs --kernel-config=/usr/src/linux-6.6.10-zen/.config --kerneldir=/usr/src/linux-6.6.10-zen/ --bootdir=/boot
grub-mkconfig -o /boot/grub/grub.cfg
```

## Extract only files from a tar archive
```
tar --wildcards -xvf file.tar filename*
```
## Incremental Rsync remote folder periodically
```
while true; do
    rsync -avz -e 'ssh' --progress root@IP:/home/folder /home/
    sleep 40
done
```
## Video Upscaler
```
#!/bin/bash

# Extract video framerate
framerate=$(ffmpeg -i 1.mp4 2>&1 | sed -n "s/.*, \(.*\) fp.*/\1/p")

# Extract frames
ffmpeg -i 1.mp4 -vf "fps=$framerate" frame_%03d.png

# Create directory for upscaled images
mkdir -p upscaled

# Enlarge frames
for i in frame_*.png; do
    realesrgan-ncnn-vulkan -i "$i" -o upscaled/"${i%.*}_upscaled.jpg"
done

# Recompile to .mkv without sound
ffmpeg -framerate $framerate -pattern_type glob -i 'upscaled/*_upscaled.jpg' -c:v libx264 video_without_sound.mkv

# Add sound from the original video
ffmpeg -i video_without_sound.mkv -i 1.mp4 -c copy -map 0:0 -map 1:1 -shortest video_name_enlarged.mkv

# Remove the frames and the upscaled images
rm frame_*.png
rm -r upscaled
rm video_without_sound.mkv

```
# Python remove special characters from song names

```
import os
import re

def clean_filename(filename):
    """Removes special characters, emojis, and keeps only standard characters."""
    allowed_chars = r"abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789._ -"  
    new_filename = re.sub(r'[^' + allowed_chars + ']', '', filename)
    return new_filename

def main():
    current_dir = os.getcwd()  # Get the current working directory

    for filename in os.listdir(current_dir):
        if filename.endswith(".mp3"):  # Check for MP3 files
            clean_name = clean_filename(filename)
            if clean_name != filename:  # Rename only if changes are needed
                old_filepath = os.path.join(current_dir, filename)
                new_filepath = os.path.join(current_dir, clean_name)
                os.rename(old_filepath, new_filepath)
                print(f"Renamed '{filename}' to '{clean_name}'")

if __name__ == "__main__":
    main()

```
# Share folder in the local network

```
sshfs janos@192.168.0.100:/home/janos/Share /home/janos/Desktop/Share
               SSHD SERVER                         REMOTE PC
```

#Calculate fuel consumption Python script

```
# Ask for user inputs
km = float(input("Enter the KM value: "))
fuel_price = float(input("Enter the fuel price in HUF (one liter): "))

# Calculate and print the cost for each consumption value
for consumption in range(50, 71):
    consumption /= 10  # Convert to liters/100km
    cost = (consumption / 100) * km * fuel_price
    print(f"{consumption} liters/100km = {cost} HUF")

```
