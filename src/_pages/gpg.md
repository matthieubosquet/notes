---
title: GPG
breadcrumbs:
  - title: Computer Dreams
  - title: Developer Tools
---

> The GNU Privacy Guard (GPG)
>
> GnuPG is a complete and free implementation of the OpenPGP standard as defined by [RFC4880](https://www.ietf.org/rfc/rfc4880.txt) (also known as PGP).
>
> — [gnupg.org](https://gnupg.org)


## Install

```sh
$ brew install gpg
```

## List existing keys

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
