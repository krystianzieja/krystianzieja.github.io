# userauth_pubkey: key type ssh-dss not in PubkeyAcceptedKeyTypes

When using modern OpenSSH server you can encounter problems when using DSA type keys. The error represents itself with the following error message in ```auth.log``` or ```secure.log```: ```userauth_pubkey: key type ssh-dss not in PubkeyAcceptedKeyTypes```.


The best option is to migrate to RSA key type if you have to work also with older SSH servers. If you need only to work with newer SSH servers ecdsa or ed25519 key types offer better security.

However changing SSH key type is not an option for you, you have to modify SSH configuration in file ```/etc/ssh/sshd_config``` by appending below line:

```
PubkeyAcceptedKeyTypes=+ssh-dss
```

and reload sshd service.

```
systemctl reload sshd
```
