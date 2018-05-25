# SSH Notes #


## Articles/Resources ##

- http://blog.trackets.com/2014/05/17/ssh-tunnel-local-and-remote-port-forwarding-explained-with-examples.html
- http://lifepluslinux.blogspot.com/2017/01/look-before-you-paste-from-website-to.html






## Sign / Verify ##


```
openssl genrsa -out private.pem 2048
openssl rsa -in private.pem -out public.pem -outform PEM -pubout
```

```
  echo -n "data-to-sign" | openssl dgst -RSA-SHA256 -sign private.pem > signed
  base64 signed
```

```
  openssl dgst -RSA-SHA256 -verify public.pem -signature signed
```


ssh-keygen 


## Upload SSH Keys ##


```
  ssh-keygen -t rsa -b 4096 -f ~/.ssh/{mykey} -P ''
  mv !!:6 !!:6.key

  ssh-copy-id -i ~/.ssh/{mykey} {user}@{hostname}
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


## Control Path ##


List keys in SSH Agent
```
ssh-add -L
```


Setup control path
```
mkdir -p ~/.ssh/cp
chmod 700 ~/.ssh/cp
```


### ~/.ssh/config ###


```
  # Protected Server
  Host server01
  	ForwardAgent yes
    HostName server01.domain.com
  	ProxyCommand ssh -A -q -W %h:%p jumpbox01
  
  # Jumpbox
  Host jumpbox01
  	ControlMaster auto
  	ControlPath ~/.ssh/cp/%r@%h:%p
  	ControlPersist 5m
  	ForwardAgent yes
    HostName jumpbox01.domain.com
```



```
ssh -O check jumpbox01
ssh -O stop jumpbox01
```



# Keys #


Generate a self-signed certificate
```
openssl req -x509 -new -newkey rsa:2048 -nodes -subj '/C=US/ST=California/L=San Francisco/O=JankyCo/CN=Test Identity Provider' -keyout idp-private-key.pem -out idp-public-cert.pem -days 7300
```

Convert DER to PEM
```
openssl x509 -inform der -in to-convert.der -out converted.pem
```

Convert Cert to Public Key
```
openssl x509 -pubkey -noout -in cert.pem > pub.key
```