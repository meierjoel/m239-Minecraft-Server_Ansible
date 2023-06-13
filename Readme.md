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

Schritt 7: Ansible Repository erstellen und installieren
ctrl+d klicken
sudo apt-add-repository ppa:ansible/ansible
Immer auf Enter klicken
sudo apt update
sudo apt install ansible
Mit "y" bestätigen und immer weiter klicken

Schritt 8: Ansible Host File anpassen
sudo nano /etc/ansible/hosts
zu unters die Zeilen aus dem hier abgelegten Ansibel Host File einfügen und speichern.

Schritt 9:  Testen, sind die Hosts vorhanden und erreichbar?
ansible-inventory --list -y 
Listet alle Host auf, man sollt unter dem server 1 den localhost sehen.
ansible all -m ping -u root 
Pingt alle Hosts an. Wenn Grün, dann ist alles ok. Ansonsten muss die SSH verbindung vom Root zum Localhost erneut getestet werden.

Schritt 10: ADHock Commands testen (Disk usage & Install Module Vim)
ansible all -a "df -h" -u root
ansible all -m apt -a "name=vim state=latest" -u root
Sollte beides ohne Probleme funktionieren.

Schritt 11: Pfad für Ansible Playbook erstellen
sudo mkdir /ansible
cd /ansible/

Schritt 12: Ansible Minecraft Playbook erstellen
sudo nano minecraft.yaml
Hier nun den Inhalt aus dem File Anisble Minecraft Playbook einfügen

Schritt 13: Pfad erstellen für Systemdeamon
sudo mkdir /ansible/templates
cd templates/

Schritt 14: Systemdeamon erstellen (File)
sudo nano minecraft.service.j2
Hier den Inhalt des Minecraft.service.j2 File einfügen.

Schritt 15: 





