# m239-Minecraft-Server_Ansible
How to Set UP a Minecraft Server with Ansible under Ubuntu 22.04 with a Systemdeamon on ine server

Voraussetzungen: 
Funktionierendes Netzwerk (Bridged bei VM)
Ubuntu Server 2022 (Ubuntu 22.04.2 LTS) bei installation inkl. SSH

Info:
Wenn User erwähnt immer den eigenen User benutzen!
Falls IP-Adresse vorkommen immer eigene Adresse benutzen!


Anleitung:
Schritt 1: IP-Adresse des Servers herausfinden
ip a

Schritt 2: SSH Verbindung auf den Ubuntu server herstellen von Windows Terminal (CMD)
ssh user@172.20.10.2
Mit "yes" bestätigen und pw des users eingeben.

Schritt 3: SSH-Key generieren
ssh-keygen
Immer auf Enter klicken bis durch.

Schritt 4: SSH-Key kopieren und beim Rootuser ablegen.
sudo cp /home/user/.ssh/id_rsa.pub /root/.ssh/authorized_keys
Bei user eigener User angeben danach noch passswort wegen sudo Befehl.

Schritt 5: SSH-Service Neustarten
sudo systemctl restart ssh

Schritt 6: SSH-Verbindung von Root auf den Localhost
ssh root@localhost
Sollte funktionieren ansonsten rebooten und erneut versuchen.

Schritt 7: 

