# Installation
The general installation on Arch is handled by `makepkg`:

```shell
makepkg -si
```

## Slater-Koster files
To really use DFTB+, you also need the Slater-Koster files. A full archive 
of publicly available SK-files can be found [here](https://dftb.org/fileadmin/DFTB/public/slako-unpacked.tar.xz).
Retrieve and unpack them into the `/opt/dftb+` installation directory with the following commands:

```shell
cd /tmp
curl -O 'https://dftb.org/fileadmin/DFTB/public/slako-unpacked.tar.xz'
xz -d slako-unpacked.tar.xz
tar xf slako-unpacked.tar
sudo cp -r slako /opt/dftb+/.
```
