# LaTeX in WSL2

## Install Tex Live

ref: [TEX Wiki](https://texwiki.texjp.org/?Linux#texliveinstall)

TeX Live also can be installed from apt, `texlive-full`, but not recommended due to not being latest

Install to `/opt` for applications installing by wget

### Update

```sh
$ sudo tlmgr update --self --all
```
downgrade from backup if problem occurs by updating

backup is at `/usr/local/texlive/????/tlpkg/backups`

`????` will be `2021` in 10/25/2021
```sh
$ sudo tlmgr restore (package_name) (revision_number)
```

## Use VScode
ref: read random websites
