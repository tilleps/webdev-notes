# GPG #


gpg --list-keys


Encrypt
```
gpg --yes --output "config/env/development.env.gpg" --encrypt `cat "config/recipients/development.txt" | xargs -I '{}' echo "--recipient" {}` "config/env/development.env"
```


Encrypt (Non-interactive mode)
```
gpg --yes --batch --passphrase="passphrase" -c filename.txt
```

Decrypt (Non-interactive mode)
```
gpg --yes --batch --passphrase="passphrase" filename.txt
```



skipped: Unusable public key


Solution #1:
  gpg --import path/to/key
  gpg --edit-key "MY KEY ID" trust
  



Expired:
```
gpg --edit-key "1A1F9E66" expire
```


## Create ##

gpg --gen-key


## Import (Public) Key ##

```
gpg --import key.asc
```


## Revoke ##
gpg --output revoke.asc --gen-revoke "1A1F9E66"

