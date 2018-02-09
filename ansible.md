# Ansible #


## Troubleshooting ##


```Could not open a connection to your authentication agent```

```
eval "$(ssh-agent)"
```

or 

```
exec ssh-agent bash
```

or (ansible friendly)

```
ssh-agent sh -c 'ssh-add /path/to/key'
```
