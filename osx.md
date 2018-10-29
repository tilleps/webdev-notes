### OSX Notes ###


### Keychain ###

-s service
-l label
-w password


```bash
security add-generic-password -a ${USER} -s myfirstaccount -w mypassword databases.keychain-db

security find-generic-password -s myfirstaccount -w databases.keychain-db

security delete-generic-password -s myfirstaccount databases.keychain-db
```


### Open New Terminal Tab via Command Line ###

```bash
osascript -e 'tell application "Terminal" to activate' -e 'tell application "System Events" to tell process "Terminal" to keystroke "t" using command down'
```


### Troubleshooting ###

Problem:
```bash
Agreeing to the Xcode/iOS license requires admin privileges, please re-run as root via sudo.
```

Solution:
```bash
sudo xcodebuild -license accept
```



Problem: "SSL certificate problem: Invalid certificate chain"
Solution: https://support.apple.com/en-ca/HT203811



Screenshot Awesomeness (screenshot regions): `cmd+shift+4` then press `spacebar`


