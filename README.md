# Client project to use the go-microservices personal project.

```
yarn serve
```

### Compiles and minifies for production
```
yarn build
```

### Lints and fixes files
```
yarn lint
```



### Run on docker
```
sudo docker run --name vue-app -w /app  -v /app:/app -p 8080:8080 -d node /bin/bash -c "yarn serve"
```
