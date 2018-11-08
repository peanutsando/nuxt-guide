install pm2 in global
```
npm i -g pm2
```

start command
```
pm2 start npm --name "your-app-alias" -- start
```

restart command (after re-build)
```
pm2 restart your-app-alias
```
