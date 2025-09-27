# Optimizing Qidi Q2 processes

*V2, updated 18/09/2025*

### I went through the Q2 process list and noted these process as most likely useless for normal printer operation

- algo_app - Qidi service, likely needed by Qidi app / AI detection

- QIDILink-client.service - Qidi service , likely needed by Qidi app / AI detection

- bluetoothd - Bluetooth service 

- pulseaudio - PulseAudio 

- lightdm - Display manager (I don't think this is needed for Klipper display) 

- xl2tpd - XL2TP Daemon (VPN services, might be used by Qidi Link? 

- triggerhappy - "a lightweight hotkey daemon", I don't think this is needed since the printer has no buttons

- strongswan - VPN stuff, probably not need (app might need it)

- openvpn - VPN, probably not needed (app might need it)

### Might be needed 

- packagekitd - Package manager - two reports that it is needed when installing new software and mods

### Verified to be needed for normal printer operation:

- QD_Q2 - needed for touchscreen, probably not needed if you run headless

- polkitd - PolicyKit, needed by Moonraker

## How to disable useless processes 

I disabled these: 

```
sudo systemctl disable --now algo_app.service
sudo systemctl disable --now QIDILink-client.service
sudo systemctl disable --now strongswan-starter.service  
sudo systemctl disable --now openvpn.service
sudo systemctl disable --now triggerhappy.service
systemctl --user disable --now pulseaudio.service
systemctl --user disable --now pulseaudio.socket
sudo systemctl disable --now polkit
sudo systemctl disable --now bluetooth
sudo systemctl disable --now lightdm
sudo systemctl disable --now packagekit
sudo systemctl disable --now xl2tpd
```

After reboot some came back so I masked them: PulseAudio, PackageKit, QIDILink, pulseaudio

```
sudo systemctl mask packagekit
sudo systemctl mask QIDILink-client.service
systemctl --user mask pulseaudio.service
systemctl --user mask pulseaudio.socket
```

### Results

I have now tested these for multiple prints without negative consequences. Fluidd interface feels noticeably faster. 

Resource usage 10 minutes after boot:

```
 free -h && echo "Load: $(uptime | awk -F'load average:' '{print $2}')" && echo "Processes: $(ps aux | wc -l)"
               total        used        free      shared  buff/cache   available
Mem:           487Mi       179Mi        21Mi       0.0Ki       285Mi       293Mi
Swap:             0B          0B          0B
Load:  0.64, 0.51, 0.38
Processes: 122
```

<img width="1736" height="826" alt="image" src="https://github.com/user-attachments/assets/5700d53d-3f07-4aac-90b4-763e63ae18e5" />


