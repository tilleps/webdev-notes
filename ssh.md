# SSH Notes #


## Articles/Resources ##
- http://blog.trackets.com/2014/05/17/ssh-tunnel-local-and-remote-port-forwarding-explained-with-examples.html




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
