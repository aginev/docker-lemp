###Issue certificate for the domain

```bash
cd ./.docker/nginx
mkcert -key-file key.pem -cert-file cert.pem docker-lemp.test "*.docker-lemp.test"
```

###Build containers
```bash
docker-composer build
```

###Start containers
```bash
docker-composer up -d
```
