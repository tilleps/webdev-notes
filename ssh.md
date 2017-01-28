# SSH Notes #


## Articles/Resources ##
- http://blog.trackets.com/2014/05/17/ssh-tunnel-local-and-remote-port-forwarding-explained-with-examples.html
- http://lifepluslinux.blogspot.com/2017/01/look-before-you-paste-from-website-to.html



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