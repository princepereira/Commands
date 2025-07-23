## Rebasing existing branch in git with upstream ##

```
git remote add upstream https://github.com/kubernetes/kubernetes.git

git pull upstream master -f

git push origin master -f
```


## Fetching a new upstream branch (release-1.32) for the first time ##

```
git fetch upstream release-1.32

git checkout upstream/release-1.32

git checkout -b release-1.32

git push origin release-1.32
```

## Revert last commit in github ##

```
git reset --hard HEAD~1

git push origin main --force
```

## Gpg failed to sign the data ##
**Error**
```
error: gpg failed to sign the data:
gpg: skipped "ppereira@microsoft.com": No secret key
[GNUPG:] INV_SGNR 9 ppereira@microsoft.com
[GNUPG:] FAILURE sign 17
gpg: signing failed: No secret key
```
**Solution :**
```
Install gpg from : https://gpg4win.org/download.html
```
```
gpg --version
```
```
git config --global gpg.program "C:/Program Files (x86)/GnuPG/bin/gpg.exe"
```
```
Get-Command gpg
```
Check secret keys present. If present use the secret key. If not present create a new one.
```
gpg --list-secret-keys --keyid-format LONG
```
Create a new key if not present
```
gpg --full-generate-key
```
```
Please select what kind of key you want: (1) RSA and RSA
What keysize do you want? (3072): 4096
Please specify how long the key should be valid: 1y
Real name: Prince Pereira
Email address: ppereira@microsoft.com
Comment: ""
Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit?: O
```
Once the key is created, get the key using list command. Value coming after "rsa4096/" is the key.
Sign the git
```
git config --global user.signingkey <Key/Email ID>
```
```
git config --global commit.gpgsign true
git config --global gpg.program "gpg"
```
Sign the commit
```
git commit -S --amend
```
