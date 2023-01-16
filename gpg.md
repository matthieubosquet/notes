# GPG

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
