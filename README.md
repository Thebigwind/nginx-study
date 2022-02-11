# nginx-study
nginx-study 笔记

me@localhostt ~ % brew install nginx
==> Downloading https://ghcr.io/v2/homebrew/core/pcre/manifests/8.45
######################################################################## 100.0%
==> Downloading https://ghcr.io/v2/homebrew/core/pcre/blobs/sha256:fb2fefbe1232706a603a6b385fc37253e5aafaf3536cb68b828ad1940b95e601
==> Downloading from https://pkg-containers.githubusercontent.com/ghcr1/blobs/sha256:fb2fefbe1232706a603a6b385fc37253e5aafaf3536cb68b828ad1940b95e601?se=2022-01-08T05%3A45%3
######################################################################## 100.0%
==> Downloading https://ghcr.io/v2/homebrew/core/nginx/manifests/1.21.1
######################################################################## 100.0%
==> Downloading https://ghcr.io/v2/homebrew/core/nginx/blobs/sha256:8ba34676e573272aa1f73d4dcf6bfddbaa69746a92bf812f6760baf13ddf93dc
==> Downloading from https://pkg-containers.githubusercontent.com/ghcr1/blobs/sha256:8ba34676e573272aa1f73d4dcf6bfddbaa69746a92bf812f6760baf13ddf93dc?se=2022-01-08T05%3A45%3
######################################################################## 100.0%
==> Installing dependencies for nginx: pcre
==> Installing nginx dependency: pcre
==> Pouring pcre--8.45.big_sur.bottle.tar.gz
🍺  /usr/local/Cellar/pcre/8.45: 204 files, 5.8MB
==> Installing nginx
==> Pouring nginx--1.21.1.big_sur.bottle.tar.gz
==> Caveats
Docroot is: /usr/local/var/www

The default port has been set in /usr/local/etc/nginx/nginx.conf to 8080 so that
nginx can run without sudo.

nginx will load all files in /usr/local/etc/nginx/servers/.

To have launchd start nginx now and restart at login:
brew services start nginx
Or, if you don't want/need a background service you can just run:
nginx
==> Summary
🍺  /usr/local/Cellar/nginx/1.21.1: 25 files, 2.2MB
==> Caveats
==> nginx
Docroot is: /usr/local/var/www

The default port has been set in /usr/local/etc/nginx/nginx.conf to 8080 so that
nginx can run without sudo.

nginx will load all files in /usr/local/etc/nginx/servers/.

To have launchd start nginx now and restart at login:
brew services start nginx
Or, if you don't want/need a background service you can just run:
nginx
me@localhostt ~ %
