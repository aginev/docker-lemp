### Add domain into the hosts file to be able to access it
```bash
sudo vim /etc/hosts
```

Add following line into the file
```
127.0.0.1   docker-lemp.test
```

### Issue certificate for the domain

```bash
cd ./.docker/nginx
mkcert -key-file key.pem -cert-file cert.pem docker-lemp.test "*.docker-lemp.test"
```

### Build containers
```bash
docker-composer build
```

### Start containers
```bash
docker-composer up -d
```

### Xdebug

Note that PHP and Xdebug are not able to work on same port so use port 9003 at PhpStorm
