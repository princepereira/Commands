### Cleanup all unwanted docker images and volumes from WSL ###
```
wsl -d docker-desktop

docker system prune -a --volumes
```
