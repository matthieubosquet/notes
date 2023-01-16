---
title: GPG
---

> The GNU Privacy Guard
>
> GnuPG is a complete and free implementation of the OpenPGP standard as defined by [RFC4880](https://www.ietf.org/rfc/rfc4880.txt) (also known as PGP).
>
> – https://gnupg.org/


## Install

```sh
$ brew install gpg
```

## Check existing keys

```sh
$ gpg --list-secret-keys --keyid-format=long
```

## Generate key

```sh
$ gpg --full-generate-key
```

## Export key

```sh
$ gpg --armor --export <GPG_KEY_ID>
```

## Install PINentry

```sh
$ brew install pinentry-mac
$ echo "pinentry-program $(which pinentry-mac)" >> ~/.gnupg/gpg-agent.conf
$ gpgconf --kill gpg-agent
```
