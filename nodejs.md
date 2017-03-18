# Node #

## Resources ##

- (Node Inspector Manager)[https://chrome.google.com/webstore/detail/nim-node-inspector-manage/gnhhdgbaldcilmgcpfddgdbkhjohddkj?hl=en-US]
- (Folder Structure)[https://blog.risingstack.com/node-js-project-structure-tutorial-node-js-at-scale/]
- (JSend Response Format)[https://labs.omniti.com/labs/jsend]


## Notes ##

Use exact version of dependencies:
```
npm shrinkwrap
```


## Debugging ##

```node --inspect index.js```
```node --inspect --debug-brk index.js```

Finding all environment variables used:  ```grep -rhoe 'process.env.[^, ]\+' --exclude-dir=node_modules .```


## Testing ##

http://www.node-tap.org/basics/


## Linting ##

http://jscs.info/


## Modules Installing ##


Install module from git repo
```
npm install git+https://repodomain/project.git
```