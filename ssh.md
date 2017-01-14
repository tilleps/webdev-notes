# SSH Notes #


`git clone gitlab:user/repo.git`

```ssh
Host gitlab
  User git
  HostName gitlab.com
  IdentityFile ~/.ssh/gitlab.pem
#  IdentitiesOnly yes
```


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