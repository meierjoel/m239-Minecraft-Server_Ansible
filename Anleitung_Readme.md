# M239-Minecraft-Server_Ansible
How to Set UP a Minecraft Server (1.19.4) with Ansible under Ubuntu 22.04 with a Systemdeamon on localhost


## Voraussetzungen
Funktionierendes Netzwerk (Bridged bei VM)
Ubuntu Server 2022 (Ubuntu 22.04.2 LTS) bei installation inkl. SSH


## Info
Wenn User erwähnt immer den eigenen User benutzen!
Falls IP-Adresse vorkommen immer eigene Adresse benutzen!


## Anleitung
### Schritt 1: IP-Adresse des Servers herausfinden
```shell
ip a
```

### Schritt 2: SSH Verbindung auf den Ubuntu server herstellen von Windows Terminal (CMD)
Bei username eigener User und bei ipaddress die IP des Ubuntu Servers angeben.
```shell
ssh username@ipaddress
```
Mit "yes" bestätigen und pw des users eingeben.

### Schritt 3: SSH-Key generieren
```shell
ssh-keygen
```
Immer auf Enter klicken bis durch.

### Schritt 4: SSH-Key kopieren und beim Rootuser ablegen.
Bei username eigener User angeben danach noch passswort wegen sudo Befehl.
```shell
sudo cp /home/username/.ssh/id_rsa.pub /root/.ssh/authorized_keys
```

### Schritt 5: SSH-Service Neustarten
```shell
sudo systemctl restart ssh
```

### Schritt 6: SSH-Verbindung von Root auf den Localhost
```shell 
ssh root@localhost
```
Sollte funktionieren ansonsten rebooten und erneut versuchen.

### Schritt 7: Ansible Repository erstellen und installieren
ctrl+d klicken
```shell
sudo apt-add-repository ppa:ansible/ansible
```
Immer auf Enter klicken
```shell
sudo apt update
sudo apt install ansible
```
Mit "y" bestätigen und immer weiter klicken

### Schritt 8: Ansible Host File anpassen
```shell
sudo nano /etc/ansible/hosts
```
Zu unterst die Zeilen aus dem hier abgelegten Ansibel Host File einfügen und speichern.

### Schritt 9:  Testen, sind die Hosts vorhanden und erreichbar?
Listet alle Host auf, man sollt unter dem server 1 den localhost sehen.
```shell
ansible-inventory --list -y 
```
Pingt alle Hosts an. Wenn Grün, dann ist alles ok. Ansonsten muss die SSH verbindung vom Root zum Localhost erneut getestet werden.
```shell
ansible all -m ping -u root 
```

### Schritt 10: ADHock Commands testen (Disk usage & Install Module Vim)
```shell
ansible all -a "df -h" -u root
ansible all -m apt -a "name=vim state=latest" -u root
```
Sollte beides ohne Probleme funktionieren.

### Schritt 11: Pfad für Ansible Playbook erstellen
```shell
sudo mkdir /ansible
cd /ansible/
```

### Schritt 12: Ansible Minecraft Playbook erstellen
```shell
sudo nano minecraft.yaml
```
Hier nun den Inhalt aus dem File Anisble Minecraft Playbook einfügen.

### Schritt 13: Pfad erstellen für Systemdeamon
```shell
sudo mkdir /ansible/templates
cd templates/
```

### Schritt 14: Systemdeamon erstellen (File)
```shell
sudo nano minecraft.service.j2
```
Hier den Inhalt des Minecraft.service.j2 File einfügen und speichern.

### Schritt 15: Ansible Playbook einmalig starten
```shell
cd ..
ansible-playbook minecraft.yaml
```
Dies kann einen kurzen Moment dauern.

### Schritt 16: EULA vom Minecraftserver akzeptieren
```shell
cd /opt/minecraft_server
sudo nano eula.txt
```
Den Wert false durch true ersetzen und speichern.

### Schritt 17: Optional anpassungen an den Minecraft Rules
```shell
cd /opt/minecraft_server
sudo nano server.properties
```
### Schritt 18: Ansible Playbook starten
```shell
ansible-playbook /ansible/minecraft.yaml
```

### Schritt 19: Auf den Server vom Minecraft Launcher verbinden via IP
Also im Minecraft Launcher version 1.19.4 --> Multiplayer --> Direct Connection -->server ip (172.20.10.2)

## Fehlerbehebung
Falls der Minecraftserver nicht direkt läuft muss man den status anschauen falls nicht grün neustarten
```shell
systemctl status minecraft.service
```
Befehl zum den Service Neustarten
```shell
systemctl restart minecraft.service 
```
