
# Usage: #


Download (Normal)

```
youtube-dl [url]
```


Download audio separated video

```
youtube-dl -f 'bestvideo,bestaudio' -o '%(title)s.f%(format_id)s.%(ext)s' [url]
```



Download audio only

```
youtube-dl -f 'bestaudio[ext=m4a]' [url]
```


Download mp3 only

```
youtube-dl -f 'bestvideo[ext=mp4]' [url]
```


# Installation #

```
mkdir -p /usr/local/opt/youtube-dl/latest/bin
sudo curl -L https://yt-dl.org/downloads/latest/youtube-dl -o /usr/local/opt/youtube-dl/latest/bin/youtube-dl
sudo ln -s /usr/local/opt/youtube-dl/latest/bin/youtube-dl /usr/local/bin/youtube-dl
sudo chmod a+rx /usr/local/bin/youtube-dl
```

Update

```
sudo youtube-dl -U
```

Convert to mp3

```
ffmpeg -i "file.m4a" -acodec libmp3lame -ab 128k "file.mp3" 
```


```
function m4atomp3() {
  local f=$(basename "$*" .m4a).mp3  
  ffmpeg -i "$1" -acodec libmp3lame -ab 128k "${f}"
}
```