# Trouble Shooting

```text
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0777 for 'filename.pem' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "filename.pem": bad permissions
```

If you get this error you might have to change the permission on the directory with you `.pem` file with `sudo chmod 700 dir`. 

