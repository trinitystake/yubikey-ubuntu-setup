# <img src="https://user-images.githubusercontent.com/114076168/199712971-76da2ac1-7eb3-4013-888b-cc83d1d9578c.png" width="35" height="35"> YubiKey Ubuntu Setup Guide [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2Fp4privacy%2Fyubikey-ubuntu-setup&count_bg=%230000ff&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)

This quick guide is about how to setup your YubiKey on Linux Ubuntu

First of all we want to check if Ubuntu recognizes our YubiKey.
Insert your YubiKey, open a terminal and type:
```bash
gnu --card-status
```

If you get this error, it means that the scdaemon who manages the smart card is missing
```diff
gpg: error getting version from 'scdaemon': No SmartCard daemon
gpg: OpenPGP card not available: No SmartCard daemon
```

Then you have to install these 5 packages if you want to make sure that your YubiKey always work.
Enter the following command to see if there are any of them already installed
```bash
apt list scdaemon yubikey-manager libpam-yubico libpam-u2f libu2f-udev
```
The output may be something like this:
```diff
Listing... Done
libpam-u2f/jammy,jammy,jammy,now 1.1.0-1.1build1 amd64
libpam-yubico/jammy,jammy,jammy,now 2.26-1.1build1 amd64
libu2f-udev/jammy,jammy,jammy,jammy,jammy,jammy,now 1.1.10-3build2 all [installed]
scdaemon/jammy-security,jammy-security,now 2.2.27-3ubuntu2.1 amd64
scdaemon/jammy-security,jammy-security 2.2.27-3ubuntu2.1 i386
yubikey-manager/jammy,jammy,jammy,jammy,jammy,jammy,now 4.0.7-1 all
```

This tells us that we have only 1 package installed out of the 5 required (libu2f-udev)
Let's install the remaining ones:
```bash
sudo nala install scdaemon yubikey-manager libpam-yubico libpam-u2f libu2f-udev
```

Reboot the system (recommended):
```bash
sudo reboot
```

Now try again to get the YubiKey content showed:
```bash
gnu --card-status
```

If you get the following output, it means that it works!
```diff
Reader ...........: 1050:0407:X:0
Application ID ...: D1506558240103040824016558934000
Application type .: OpenPGP
Version ..........: 3.4
Manufacturer .....: Yubico
Serial number ....: 20655895
Name of cardholder: [not set]
Language prefs ...: [not set]
Salutation .......: 
URL of public key : [not set]
Login data .......: [not set]
Signature PIN ....: not forced
Key attributes ...: rsa2048 rsa2048 rsa2048
Max. PIN lengths .: 127 127 127
PIN retry counter : 3 0 3
Signature counter : 0
KDF setting ......: off
Signature key ....: [none]
Encryption key....: [none]
Authentication key: [none]
General key info..: [none]
```

Start the sudo pcscd service:
```bash
sudo systemctl start pcscd.service
```

One of the packages you installed is the YubiKey Manager.
Now let's see if our YubiKey is displayed correctly
```bash
ykman list
```

The output should be something like:
```diff
YubiKey 5C NFC (5.4.3) [OTP+FIDO+CCID] Serial: 123456789
```

Well done, now your YubiKey is ready to be used!
