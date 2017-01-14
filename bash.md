# Bash Notes #


http://www.davidpashley.com/articles/writing-robust-shell-scripts/



Check Operating System and Version
```
lsb_release -a
```


```
sudo usermod -a -G groupName userName
```



Convert to mp3
```
ffmpeg -i "file.m4a" -acodec libmp3lame -ab 128k "file.mp3" 
```