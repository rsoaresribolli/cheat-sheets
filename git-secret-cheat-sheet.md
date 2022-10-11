# Pre-requisites

Before starting with git secret, you must set up a GPG RSA key pair.
This can be done by following the [dev dungeon tutorial](https://www.devdungeon.com/content/gpg-tutorial).

# Initialize git secret repository

```bash
git secret init
```

# Add user to git secret

## 1. Chek if their public key is imported
```bash
gpg --list-keys
```
### If it is not imported, follow the steps to do it:
### 1.1. Export their key
```bash
gpg --armor --export their@email.id > public_key.txt
```
### 1.2. Import their key into your GPG keyring
```bash
gpg --import public_key.txt
```

## 2. Add the new person to secrets
```bash
git secret tell their@email.id
```
If the new person is yourself, you can simply do
```bash
git secret tell -m
```
The `-m` flag uses the `git config user.email`.

### 2.1. Additionally, you may remove the other person's public key from your gpg keyring
```bash
gpg --delete-keys their@email.id
```

## 3. Re-encrypt files so the new user can read them
```bash
git secret reveal
git secret hide -d
```

The `-d` option deletes the unencrypted files.

# Managing secrets

## 1. Adding to secret
```bash
git secret add <filenames...>
```

## 2. Hiding added files
```bash
git secret hide
```
Or, if you want to delete the original files
```bash
git secret hide -d
```

## 3. Revealing secrets
To reveal all files in secret
```bash
git secret reveal
```

To print secret content to stdout
```bash
git secret cat <filepath>
```

# Managing GPG Keys

## List public keys
```bash
gpg --list-keys
```

## List private keys
```bash
gpg --list-secret-keys
```
