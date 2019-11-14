# manjaro-headless
Use manjaro linux as a headless server

# Step 1
Install manjaro mininmal with xfce

# Step 2
Enable ssh
```sudo systemctl enable sshd.service; sudo systemctl start sshd.service```

# Step 3
Make it headless
```sudo pacman -Rs xfce4 gtkhash-thunar libxfce4ui mousepad orage thunar-archive-plugin thunar-media-tags-plugin xfce4-battery-plugin xfce4-clipman-plugin xfce4-pulseaudio-plugin xfce4-screenshooter xfce4-whiskermenu-plugin xfce4-whiskermenu-plugin xfce4-xkb-plugin parole xfce4-notifyd```
```sudo pacman -Rs lightdm light-locker lightdm-gtk-greeter lightdm-gtk-greeter-settings modemmanager```

Copy your ssh keys to the server now

```mkdir ~/.ssh #needed for copying key```
```cat ~/.ssh/id_rsa.pub | ssh root@example.com 'cat - >> ~/.ssh/authorized_keys'```

# Step 4 (Optional)
Add goodies and secure the beast.
```
sudo pacman -Sy docker docker-compose glances htop bmon jq whois yay ufw fail2ban

cat <<EOT > /etc/fail2ban/jail.d/sshd.local
[sshd]
enabled   = true
filter    = sshd
banaction = ufw
backend   = systemd
maxretry  = 5
findtime  = 1d
bantime   = 52w
EOT
sudo systemctl start fail2ban.service
sudo systemctl enable fail2ban.service
```
