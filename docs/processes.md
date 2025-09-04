I went through the Q2 process list and noted these process as most likely useless (NOT LONG-TERM TESTED!):

bluetoothd - Bluetooth service 

pulseaudio - PulseAudio 

lightdm - Display manager (I don't think this is needed for Klipper display) 

packagekitd - Package manager (I think this is only neede for desktop GUI update manager?)

polkitd - PolicyKit (I don't think this is needed without desktop GUI) [correction: it is needed by Moonraker]

xl2tpd - XL2TP Daemon (VPN services, might be used by Qidi Link? 

(this is in addition to the previously discussed algo_app)

I stopped these: 

```
sudo systemctl stop --now bluetooth
sudo systemctl stop --now lightdm
sudo systemctl stop --now packagekit
sudo systemctl stop --now polkitd
sudo systemctl stop --now xl2tpd
```

I disabled these: 

```
systemctl --user disable --now pulseaudio.service
systemctl --user disable --now pulseaudio.socket
sudo systemctl disable --now polkit
```

Nothing seemed to break so I disabled these too:

```
sudo systemctl disable --now bluetooth
sudo systemctl disable --now lightdm
sudo systemctl disable --now packagekit
sudo systemctl disable --now xl2tpd
```

Nothing seemed to break so I rebooted and I get no errors. 

After reboot these came back PulseAudio, PackageKit, PolicyKit. I masked them:

```
sudo systemctl mask packagekit
sudo systemctl mask polkit
systemctl --user mask pulseaudio.service
systemctl --user mask pulseaudio.socket
```

I rebooted again. I checked that everything I wanted is now really disabled. Found that Moonraker warns about PolKit missing. I unmasked that. 

```
sudo systemctl unmaskmask polkit
```

Rebooted and now everything seems ok.
