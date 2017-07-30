# Basic EdgeOS Setup

## Out of the Box
- Plug an ethernet Cable into eth0
- Configure your PC/Notebook Ethernet Card to use the IP Address 192.168.1.5/24. Set no default Gateway
- Open a Webbrowser and go to https://192.168.1.1
- Login with the default Credentials: ubnt/ubnt
- Update your Firmware: Click on System (on the bottom left) and scroll down to *Upgrade System Image*
- Download the latest Image from the Ubiquitis website and upload the image to the Router

### Firmware Upgrade via CLI
Go to the Ubiquiti's Firmware Website and copy the URL

```
add system image https://dl.ubnt.com/firmwares/edgemax/v1.9.7/ER-e200.v1.9.7.5001803.tar
show system image
reboot
```

## After Firmware Update
- Change the default Username / Password: Log back in and click on Users. Create a new User (be carefull with uper and lowercase). Logout and Log back in with the newly created User
- Set an default Gateway and default DNS Server: click on System and fill in the blanks :D
