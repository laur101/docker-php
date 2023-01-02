# draft-docker-php

The PoC for https://github.com/exozet/docker-php-fpm/wiki/Draft-for-new-Structure

## Test it

1. Clone the repo
2. Run for php 8.1

```shell
$ ./build_images.sh exozet/draft-docker-php:8.1.13 3.17.0 8.1.13 php81 1.28.0 2.4.54
```

or run for php 8.0

````shell
$ ./build_images.sh exozet/draft-docker-php:8.0.26 3.16.3 8.0.26 php8 1.26.1 2.4.54
````

If you lack specific emulators (for running the multiarch build), install:

```shell
docker run --privileged --rm tonistiigi/binfmt --install arm64,riscv64,arm,386
```

3. Create a folder public with an index.php

```shell
$ mkdir public
$ echo '<?php phpinfo();' > public/index.php
```

4. Run the NGINX Unit Version with:

```shell
$ docker run --rm -p 8080:8080 -v `pwd`/public:/usr/src/app/public -it  exozet/draft-docker-php:8.1.13 unitd --no-daemon --user www-data --group www-data --log /dev/stdout
```

and open http://localhost:8080 to see phpinfo unit.

```shell
$ ab -n 1000 -c 20 http://localhost:8080/
Requests per second:    213.56 [#/sec] (mean)
Time per request:       93.651 [ms] (mean)
```

5. Run the Apache2 Version with:

```shell
$ docker run --rm -p 8080:8080 -v `pwd`/public:/usr/src/app/public -it  exozet/draft-docker-php:8.1.13 httpd -DFOREGROUND
```

and open http://localhost:8080 to see phpinfo on apache2.

Short benchmark:

```shell
$ ab -n 1000 -c 20 http://localhost:8080/
Requests per second:    551.83 [#/sec] (mean)
Time per request:       36.243 [ms] (mean)
```