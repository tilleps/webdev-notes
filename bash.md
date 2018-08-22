# Bash Notes #

https://www.netmeister.org/blog/passing-passwords.html
http://www.davidpashley.com/articles/writing-robust-shell-scripts/


export HISTCONTROL=ignoreboth:erasedups
Add to .bashrc to avoid duplicate entries


Check Operating System and Version
```
lsb_release -a
```


```
sudo env "PATH=$PATH" command
```

Loading Env files with spaces and exclude comments
```
eval `cat ${ENV_DECRYPTED_PATH} | grep -v ^#` nodemon ${ENTRY_POINT} 
```

WITHOUT EVAL
```
env -S "$(cat ${APP_ENV_FILE} | grep -v ^# | grep -v '^$')" "$@"
env -S "$(cat ${APP_ENV_FILE} | grep -v ^# | grep -v '^$')" node index.js
```

Loading environment variables from file
```
# Works for OSX (but not Ubuntu ;_;)
#env -S "$(cat ${APP_ENV_FILE} | grep -v ^# | grep -v '^$')" "$@"

# Works on Ubuntu (not on OSX ;_;)
#env "$(cat ${APP_ENV_FILE} | grep -v ^# | grep -v '^$' | xargs -d "\n" env)" "$@"

# Works for both OSX and Ubuntu, but uses eval ;_;
eval $(cat ${APP_ENV_FILE} | grep -iE "^[a-z_0-9]+=(\".*\"|'.*')$" | grep -v ^# | grep -v '^$') "$@"
```


```
sudo usermod -a -G groupName userName
```


Convert to mp3

```
ffmpeg -i "file.m4a" -acodec libmp3lame -ab 128k "file.mp3" 
```


Outputs
/dev/fd/0  stdin
/dev/fd/1  stdout
/dev/fd/2  stderr


```
ls -l * 2> /var/tmp/unaccessible-in-spool
ls -l * > /var/tmp/spoollist 2>&1
ls -l * &> /var/tmp/spoollist 2>&1
ls -l * 2 >& 1 > /var/tmp/spoollist

2>/dev/null
&> FILE
 > FILE 2>&1
```


Deploy
```
rsync -Phurv --exclude='.git/' /source/path remote-server:/path/to/destination
```


## Shortcuts ##

```
  Ctrl+R      search history
              - then Ctrl-r again to show next match
              - then Tab to show all options

  Ctrl+A      move to beginning of line
  Ctrl+E      move to end of line
  Ctrl+K      cut from position to end of line
  Ctrl+U      cut from position beginning of line

  Esc-D       cut/delete by a word (forward)
  Ctrl+W      cut/delete by a word (backward)
  Ctrl+Y      paste
  
  Esc-F       move forward by a word
  Esc-B       move backward by a word
  Ctrl+F      move foward a character
  Ctrl+B      move backward a character
```
  
```
  cd -        change to last dir
  pushd <dir> mark current dir and go to <dir>
  popd        go to marked dir 

  less        better than cat, doesn't flood screen, same keys
  find        find files, e.g. find / -name <filename>
  !!          last command, e.g. sudo !!
  $!          PID of last script
  $$          PID of current script
  $#          Number of arguments of script
  $?          Exit code of last script
```


## Scripting Template ##




Get Input from pipe
```
INPUT=$(</dev/stdin)
```


```bash
  #!/usr/bin/env bash
  #
  # Bash by Example
  # 
  # @author Eugene Song <tilleps@gmail.com>
  # @reference http://tldp.org/LDP/Bash-Beginners-Guide/html/chap_08.html



  #
  # Check number of arguments
  #
  if [ "$#" == "2" ]; then
    echo -e "Number of arguments is 2"
  fi



  #
  # Echo Examples
  #
  function echoAction() {
    echo "Bash by Example";

    echo "Print \t \n \r"
    echo -e "Convert \t \n \r"
    echo -e "Escaped \\\t \\\n \\\r"
  }




  function inputAction() {
    echo "What is your name?"
    read inputName
  
    echo "You have typed: ${inputName}"
  }


  function exampleAction () {
    echo $"example function! $1";
  }



  case "$1" in
  
    echo)
      echoAction
    ;;
  
    example)
      exampleAction "what"
    ;;
  
    input)
      inputAction
    ;;
  
    start)
      start
    ;;

    stop)
      stop
    ;;

    status)
      status anacron
    ;;
  
    restart)
      stop
      start
    ;;

    condrestart)
      if test "x`pidof anacron`" != x; then
        stop
        start
      fi
    ;;

    *)
      echo $"Usage: $0 {example|start|stop|restart|condrestart|status}"
      exit 1

  esac
`


## IPTables ##


List Rules
```
iptables -t nat -L
```

Add Rule
```
iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 10022 -j DNAT --to 10.199.199.144:22
```

Delete Rule
```
iptables -t nat -D PREROUTING -i eth0 -p tcp --dport 10022 -j DNAT --to 10.199.199.144:22
```


## One-Liners ##

Check for clock drift on multiple servers
```
for x in {1..3}; do ssh subdomain0$x.example.com date -u; done
```