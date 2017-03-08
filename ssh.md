# SSH Notes #


## Articles/Resources ##

- http://blog.trackets.com/2014/05/17/ssh-tunnel-local-and-remote-port-forwarding-explained-with-examples.html
- http://lifepluslinux.blogspot.com/2017/01/look-before-you-paste-from-website-to.html

## Sign / Verify ##


```
openssl genrsa -out private.pem 1024
openssl rsa -in private.pem -out public.pem -outform PEM -pubout
```

```
  echo -n "data-to-sign" | openssl dgst -RSA-SHA256 -sign private.pem > signed
  base64 signed
```


## Run Script ##

Running a script on a remote server

```bash
ssh user@host 'bash -s' < /path/script.sh
```
Or

```bash
cat /path/script.sh | ssh user@host 'bash -s'
```


## Port Forwarding ##

Remote Port Forwarding

````
  ssh -nNR 80:localhost:8080 hostname &
  echo $!

  kill $!
```


Check for existing SSH tunnels

```
  ps aux | grep ssh
```


## Gitlab Setup ##
`git clone gitlab:user/repo.git`

```ssh
  Host gitlab
    User git
    HostName gitlab.com
    IdentityFile ~/.ssh/gitlab.pem
  #  IdentitiesOnly yes
```


## SSH Config ##

```
  LocalForward 5433 localhost:5432
  ProxyCommand ssh gateway -W %h:%p
  
  # Speed up ssh connections
  GSSAPIAuthentication no
  IdentitiesOnly yes
```


## Setup LDAP SSH Access ##

- https://eng.ucmerced.edu/soe/computing/services/ssh-based-service/ldap-ssh-access



GPG Multikey

```
git archive --remote=$REMOTE_URL HEAD $SECRETS_FILENAME.gpg | tar -Ox | gpg --decrypt -q 2>>$LOG_LOCATION | xargs -I{} echo "export "\{\}
```