---
title: "Nahraďte Docker Desktop pomocí programu Podman na MacOS"
date: 2021-09-02T18:09:52+02:00
---

Poslední den v srpnu nám [Docker Inc.](https://www.docker.com/blog/updating-product-subscriptions/) oznámili novinky v podmínkách používání, které se týkají Docker Desktop což je asi nejvíce používaný způsob jak pracovat s dockerem na macOS. Možná si řeknete to se týká jen těch co pracují ve velké firmě a mě to netrápí. Určitě to tak může být.

Já jsem si řekl, že mi linuxu stejně už docker nepoužíváme, pokud pracujete s kubernetes (k8s) tak tam ho také nepoužíváte. Tak jsem si říkal, zda ho vůbec potřebuje a není čas na změnu.

Projekt [Podman](https://podman.io/) existuje přes 3 roky a na linuxu se stává standardem. Jak jsme na to s macOS?

## Instalace

Jednoduše přes Homebrew

```
brew install podman
```

Pokud máte Apple Silicon [můžete zkusit pokročilou verzi](https://github.com/containers/podman/blob/main/docs/tutorials/mac_experimental.md).

Zatím totiž na M1 Apple dostanete tuto zprávu.

```
$ podman machine init

Error: due to missing upstream patches, Apple Silicon is not capable of running Podman machine yet
```

Ale řešení bude a v experimentální verzi to můžete i zkusit nebo jako já sledovat až se [issue](https://github.com/containers/podman/issues/10577) vyřeší. Myslím, že prohlášení od dockeru pomůže rychle dotáhnout podman na macOS do plně funkční verze i pro Apple Silicon.

Pokud máte Intel mac tak vše funguje takto:

```
$ podman machine init

Downloading VM image: fedora-coreos-34.20210821.1.1-qemu.x86_64.qcow2.xz: done
Extracting compressed file

$ podman machine start

INFO[0000] waiting for clients...
INFO[0000] listening tcp://0.0.0.0:7777
INFO[0000] new connection from  to /var/folders/3x/cvnp4s7j43985xrz5v7_b8sh0000gn/T/podman/qemu_podman-machine-default.sock
Waiting for VM ...
qemu-system-x86_64: warning: host doesn't support requested feature: CPUID.80000001H:ECX.svm [bit 2]
```

## Výroba docker image

Vzal jsem [Dockerfile](https://github.com/apiaryio/apiary-client/blob/master/Dockerfile) a zkusil ho vyrobit.

```
podman build .

STEP 1/4: FROM ruby:2.7-alpine
Error: error creating build container: short-name resolution enforced but cannot prompt without a TTY
```

Je potřeba malá změna a řádek

```
FROM ruby:2.7-alpine
```

nahradit plnou cestou

```
FROM docker.io/ruby:2.7-alpine
```

potom už jenom

```
$ podman build . -t apiaryio/client

STEP 1/4: FROM docker.io/ruby:2.7-alpine
Trying to pull docker.io/library/ruby:2.7-alpine...
Getting image source signatures
Copying blob sha256:38cc3b271f3a2557b8da3c47052c8429ae85bb95c7bf934debef3363d118f247
Copying blob sha256:a0d0a0d46f8b52473982a3c466318f479767577551a53ffc9074c9fa7035982e
Copying blob sha256:0461ee550e837289e2d3304edc645ab33a2472563a2012ebe8b0f140e7c990ca
Copying blob sha256:e6ea4108f61712dd7e6071e6442af225dc4adf518217ab5cede7f64d420b3189
Copying blob sha256:190e5e408e0e571c6e93189f74838da8ba10848dd579773c9554cf8047cd8c7e
Copying blob sha256:e6ea4108f61712dd7e6071e6442af225dc4adf518217ab5cede7f64d420b3189
Copying blob sha256:a0d0a0d46f8b52473982a3c466318f479767577551a53ffc9074c9fa7035982e
Copying blob sha256:38cc3b271f3a2557b8da3c47052c8429ae85bb95c7bf934debef3363d118f247
Copying blob sha256:190e5e408e0e571c6e93189f74838da8ba10848dd579773c9554cf8047cd8c7e
Copying blob sha256:0461ee550e837289e2d3304edc645ab33a2472563a2012ebe8b0f140e7c990ca
Copying config sha256:41936995941906ac5c6098e7fa98242eef31e7ac77a7fddc6271681919cca7f7
Writing manifest to image destination
Storing signatures
STEP 2/4: RUN apk add --update build-base && rm /var/cache/apk/*
fetch https://dl-cdn.alpinelinux.org/alpine/v3.14/main/x86_64/APKINDEX.tar.gz
fetch https://dl-cdn.alpinelinux.org/alpine/v3.14/community/x86_64/APKINDEX.tar.gz
(1/17) Installing binutils (2.35.2-r2)
(2/17) Installing libmagic (5.40-r1)
(3/17) Installing file (5.40-r1)
(4/17) Installing libgomp (10.3.1_git20210424-r2)
(5/17) Installing libatomic (10.3.1_git20210424-r2)
(6/17) Installing libgphobos (10.3.1_git20210424-r2)
(7/17) Installing isl22 (0.22-r0)
(8/17) Installing mpfr4 (4.1.0-r0)
(9/17) Installing mpc1 (1.2.1-r0)
(10/17) Installing gcc (10.3.1_git20210424-r2)
(11/17) Installing musl-dev (1.2.2-r3)
(12/17) Installing libc-dev (0.7.2-r3)
(13/17) Installing g++ (10.3.1_git20210424-r2)
(14/17) Installing make (4.3-r0)
(15/17) Installing fortify-headers (1.1-r1)
(16/17) Installing patch (2.7.6-r7)
(17/17) Installing build-base (0.5-r2)
Executing busybox-1.33.1-r3.trigger
OK: 208 MiB in 53 packages
--> b8d4cb31709
STEP 3/4: RUN gem install apiaryio
Successfully installed http-accept-1.7.0
Building native extensions. This could take a while...
Successfully installed unf_ext-0.0.7.7
Successfully installed unf-0.1.4
Successfully installed domain_name-0.5.20190701
Successfully installed http-cookie-1.0.4
Successfully installed mime-types-data-3.2021.0901
Successfully installed mime-types-3.3.1
Successfully installed netrc-0.11.0
Successfully installed rest-client-2.1.0
Successfully installed rack-2.2.3
Successfully installed thor-0.20.3
Successfully installed public_suffix-4.0.6
Successfully installed addressable-2.8.0
Successfully installed launchy-2.5.0
Successfully installed rb-fsevent-0.11.0
Building native extensions. This could take a while...
Successfully installed ffi-1.15.4
Successfully installed rb-inotify-0.10.1
Successfully installed listen-3.7.0
Successfully installed apiaryio-0.16.1
19 gems installed
--> 2873b169a8f
STEP 4/4: ENTRYPOINT ["apiary"]
COMMIT
--> 0aa39590808
0aa3959080878b8b3b36f1c43441a2e999495d52433a3e6f267e588f2006aa2d
```

a krásně funguje i cache

```
podman build . -t apiaryio/client
STEP 1/4: FROM docker.io/ruby:2.7-alpine
STEP 2/4: RUN apk add --update build-base && rm /var/cache/apk/*
--> Using cache b8d4cb31709d063490b247d37aacc687bd356af58d6d7425f5e0a25e98a0f76e
--> b8d4cb31709
STEP 3/4: RUN gem install apiaryio
--> Using cache 2873b169a8f01c18925479ada0377490dda2084dbd5a86fdaa7a4d96a84da0e7
--> 2873b169a8f
STEP 4/4: ENTRYPOINT ["apiary"]
--> Using cache 0aa3959080878b8b3b36f1c43441a2e999495d52433a3e6f267e588f2006aa2d
COMMIT apiaryio/client
--> 0aa39590808
Successfully tagged localhost/apiaryio/client:latest
0aa3959080878b8b3b36f1c43441a2e999495d52433a3e6f267e588f2006aa2d
```

## Závěr

Myslím, že postupně přejdeme na podman a určitě nebude problém přestat Docker Desktop používat stejně jako v Kubernetes 1.22. Doporučují video [Viktora Farcic = Docker Is Dead? What Now? How Are We Going To Live Without It?](https://www.youtube.com/watch?v=xa453MkdaAk), kde to dobře vysvětluje.


