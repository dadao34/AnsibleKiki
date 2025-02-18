# Ansible Kiki
## Exercice 1

 - Démarrez la VM ubuntu depuis le répertoire atelier-01.
 `vagrant up ubuntu`
-   Connectez-vous à cette VM.
`vagrant ssh ubuntu`
-   Rafraîchissez les informations sur les paquets.
`sudo apt update`
-   Recherchez le paquet `ansible` avec les options qui vont bien.
`apt search --names-only ansible`
```
ansible/jammy 2.10.7+merged+base+2.10.8+dfsg-1 all  
Configuration management, deployment, and task execution system  
  
ansible-core/jammy-updates 2.12.0-1ubuntu0.1 all  
Configuration management, deployment, and task execution system  
  
ansible-lint/jammy 5.4.0-2 all  
lint tool for Ansible playbooks  
  
ansible-mitogen/jammy 0.3.1-3 all  
Fast connection strategy for Ansible  
  
python-ansible-runner-doc/jammy 2.1.1-1 all  
library that interfaces with Ansible (docs)  
  
python-networking-ansible-doc/jammy 17.0.0-2 all  
OpenStack virtual network service - Ansible plugin (docs)  
  
python3-ansible-runner/jammy 2.1.1-1 all  
library that interfaces with Ansible (Python 3.x)  
  
python3-networking-ansible/jammy 17.0.0-2 all  
OpenStack virtual network service - Ansible plugin (Python 3.x)
```
-   Installez le paquet officiel fourni par la distribution.
`sudo apt install ansible`
-   Vérifiez si l’installation s’est bien déroulée.
`ansible --version`
-   Notez la version d’Ansible.
`ansible 2.10.8`
-   Déconnectez-vous et supprimez la VM.
```
exit
vagrant destroy -f ubuntu
```
## Exercice 2
- Notez la version fournie par ce dépôt tiers et comparez avec la version officielle de l’exercice précédent.
`ansible [core 2.17.8]`
La version fourni par ce dépot est plus récente. Cette  différence est du au fait que le dépot ppa est surtout utiliser par des développeur, la ou les dépots officiels, eux sont plus stable garde plus longtemps les vesion LTS.

## Exercie 3
- update du gestionnaire de packet dnf :
`sudo dnf update`
- installation de python :
`sudo dnf install python3`
`sudo dnf install python3-pip`
- installation du dépot Epel pour installer python3-venv pas présent dans le dnf de base.
`sudo dnf install epel-release`
`sudo crb enable`
`sudo dnf install python3-venv`
- création de l’environnement virtuel python 
`python3 -m venv ~/.venv/ansible`
`source ~/.venv/ansible/bin/activate`
`pip install --upgrade pip`
- installation de ansible
`pip install ansible`
`ansible --version`
```
ansible [core 2.15.13]
```

## Exercice Authentification
J'ajoute ces lignes au fichier `/etc/hosts`
```
192.168.56.20 target01.sandbox.lan target01  
192.168.56.30 target02.sandbox.lan target02  
192.168.56.40 target03.sandbox.lan target03
```
Je vérifie la connectivité de mes VM. J'utilise la commande : `for HOST in target01 target02 target03; do ping -c 1 -q $HOST; done`
```
PING target01.sandbox.lan (192.168.56.20) 56(84) bytes of data.  
  
--- target01.sandbox.lan ping statistics ---  
1 packets transmitted, 1 received, 0% packet loss, time 0ms  
rtt min/avg/max/mdev = 0.948/0.948/0.948/0.000 ms  
PING target02.sandbox.lan (192.168.56.30) 56(84) bytes of data.  
  
--- target02.sandbox.lan ping statistics ---  
1 packets transmitted, 1 received, 0% packet loss, time 0ms  
rtt min/avg/max/mdev = 1.703/1.703/1.703/0.000 ms  
PING target03.sandbox.lan (192.168.56.40) 56(84) bytes of data.  
  
--- target03.sandbox.lan ping statistics ---  
1 packets transmitted, 1 received, 0% packet loss, time 0ms  
rtt min/avg/max/mdev = 1.212/1.212/1.212/0.000 ms
```

Je collecte les clés ssh publique de mes machines : `ssh-keyscan -t rsa target01 target02 target03 >> .ssh/known_hosts`
```
# target02:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.10  
# target01:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.10  
# target03:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.10
 ```
 Je génère ma clé ssh : `ssh-keygen`
 Enfin, je distribue ma clé sur les machines cibles en renseignant le mot de passe de mon Controller à chaque fois: 
 ```
 ssh-copy-id target01
 ssh-copy-id target02
 ssh-copy-id target03
 ```
 Je ping alors mes machines avec Ansible : `ansible all -i target01,target02,target03 -m ping`
 ```
target02 | SUCCESS => {  
"ansible_facts": {  
"discovered_interpreter_python": "/usr/bin/python3"  
},  
"changed": false,  
"ping": "pong"  
}  
target01 | SUCCESS => {  
"ansible_facts": {  
"discovered_interpreter_python": "/usr/bin/python3"  
},  
"changed": false,  
"ping": "pong"  
}  
target03 | SUCCESS => {  
"ansible_facts": {  
"discovered_interpreter_python": "/usr/bin/python3"  
},  
"changed": false,  
"ping": "pong"  
}
 ```
## Exercice Configuration de base (6)
- Éditez `/etc/hosts` de manière à ce que les _Target Hosts_ soient joignables par leur nom d’hôte simple.
```
192.168.56.20 sandbox.lan.target01 target01  
192.168.56.30 sandbox.lan.target02 target02  
192.168.56.40 sandbox.lan.target03 target03
```
- Configurez l’authentification par clé SSH avec les trois _Target Hosts_.
Depuis l'host, je génère ma Clé : `ssh-keygen`. J'ajoute ensuite les machines comme hôtes ssh connus `ssh-keyscan -t rsa target01 target02 target03 >> .ssh/known_hosts` et enfin je copie ma clé ssh publique sur les machines cibles : 
```
ssh-copy-id vagrant@target01
ssh-copy-id vagrant@target02
ssh-copy-id vagrant@target03
``` 
- Installez Ansible.
Je vérifie si ansible n'est pas déja installer sur la machine control `ansible --version`. La commande m'indique qu'Ansible  n'est pas installé, je l'installe donc : `sudo apt install ansible`
- Envoyez un premier `ping` Ansible sans configuration.
`ansible all -i target01,target02,target03 -m ping`
```target03 | SUCCESS => {  
"ansible_facts": {  
"discovered_interpreter_python": "/usr/bin/python3"  
},  
"changed": false,  
"ping": "pong"  
}  
target01 | SUCCESS => {  
"ansible_facts": {  
"discovered_interpreter_python": "/usr/bin/python3"  
},  
"changed": false,  
"ping": "pong"  
}  
target02 | SUCCESS => {  
"ansible_facts": {  
"discovered_interpreter_python": "/usr/bin/python3"  
},  
"changed": false,  
"ping": "pong"  
}
```
- Créez un répertoire de projet `~/monprojet`.
`mkdir ~/monprojet`
- Créez un fichier vide `ansible.cfg` dans ce répertoire.
`touch ansible.cfg`
- Vérifiez si ce fichier est bien pris en compte par Ansible.
Je me déplace dans mon répertoire `monprojet` et j'exécute la commande `ansible --version` Dans le résultat de la commande, j'y trouve la ligne suivante : `config file = /home/vagrant/monprojet/ansible.cfg`
- Spécifiez un inventaire nommé `hosts`
	`touch hosts`
- Activez la journalisation dans `~/journal/ansible.log`.
	`mkdir ~/journal` -> `touch ~/journal/ansible.log`. Je renseigne maintenant les valeurs suivantes dans `ansible.cfg`. 
```
[defaults]  
inventory = ./hosts  
log_path = ~/journal/ansible.log
```
- Testez la journalisation.
`ansible target01,target02,target03 -m ping` -> `cat ~/journal/ansible.log`
```
target03 | SUCCESS => {  
"ansible_facts": {  
"discovered_interpreter_python": "/usr/bin/python3"  
},  
"changed": false,  
"ping": "pong"  
}  
target01 | SUCCESS => {  
"ansible_facts": {  
"discovered_interpreter_python": "/usr/bin/python3"  
},  
"changed": false,  
"ping": "pong"  
}  
target02 | SUCCESS => {  
"ansible_facts": {  
"discovered_interpreter_python": "/usr/bin/python3"  
},  
"changed": false,  
"ping": "pong"  
}
```
- Créez un groupe `[testlab]` avec vos trois _Target Hosts_. && -   Définissez explicitement l’utilisateur `vagrant` pour la connexion à vos cibles.
Je renseigne le fichier inventaire `hosts` 
```
[testlab]  
target01  
target02  
target03  
  
[testlab:vars]  
ansible_user=vagrant  
ansible_python_interpreter=/usr/bin/python3
```
- Envoyez un `ping` Ansible vers le groupe de machines `[all]`.
` ansible all -m ping `
- Définissez l’élévation des droits pour l’utilisateur `vagrant` sur les _Target Hosts_. 
  J'ajoute la ligne `ansible_become=yes` à mon inventaire.
 - Affichez la première ligne du fichier `/etc/shadow` sur tous les _Target Hosts_.
 `ansible all -a "head -n 1 /etc/shadow"`
 ```
target02 | CHANGED | rc=0 >>  
root:*:19769:0:99999:7:::  
target01 | CHANGED | rc=0 >>  
root:*:19769:0:99999:7:::  
target03 | CHANGED | rc=0 >>  
root:*:19769:0:99999:7:::
```
## Exercice Idempotence (8)
- Installez successivement les paquets `tree`, `git` et `nmap` sur toutes les cibles.
```
ansible all -m package -a "name=tree"
ansible all -m package -a "name=git"
ansible all -m package -a "name=nmap"
```
- Désinstallez successivement ces trois paquets en utilisant le paramètre supplémentaire `state=absent`.
```
ansible all -m package -a "name=tree state=absent"
ansible all -m package -a "name=git state=absent"
ansible all -m package -a "name=nmap state=absent"
```
- Copier le fichier `/etc/fstab` du _Control Host_ vers tous les _Target Hosts_ sous forme d’un fichier `/tmp/test3.txt`.
 ```
 ansible all -m copy -a "src=/etc/fstab dest=/tmp/test3.txt" 
 ```

- Supprimez le fichier `/tmp/test3.txt` sur les _Target Hosts_ en utilisant le module `file` avec le paramètre `state=absent`.
```
ansible all -m file -a "dest=/tmp/test3.txt state=absent"
```
- Enfin, affichez l’espace utilisé par la partition principale sur tous les _Target Hosts_. Que remarquez-vous ?
```
ansible all -a "df -h"
```
Je remarque que le status reste à `changed` lorsque je répète la commande.

## Exercice un serveur web simple (10)
### Écrivez trois _playbooks_ :
-   Un premier _playbook_ `apache-debian.yml` qui installe Apache sur l’hôte `debian` avec une page personnalisée _Apache web server running on Debian Linux_.
```
--- # apache-debian.yml  
- hosts: debian  
  
tasks:  
  
- name: Update package apt  
apt:  
update_cache: true  
  
- name: Install Apache  
apt:  
name: apache2  
  
- name: Upload index page  
copy:  
dest: /var/www/html/index.html  
mode: 0644  
src: ../ressources/index.html  
...
```
-   Un deuxième _playbook_ `apache-rocky.yml` qui installe Apache sur l’hôte `rocky` avec une page personnalisée _Apache web server running on Rocky Linux_.
```
--- # apache-rocky.yml  
  
- hosts: rocky  
  
tasks:  
  
- name: Update package dnf  
dnf:  
update_cache: true  
  
- name: install Apache  
dnf:  
name: httpd  
  
- name: start Apache service  
service:  
name: httpd  
state: started  
  
- name: Upload index page  
copy:  
dest: /var/www/html/index.html  
src: ../ressources/index.html  
mode: 0644  
...
```
-   Un troisième _playbook_ `apache-suse.yml` qui installe Apache sur l’hôte `suse` avec une page personnalisée _Apache web server running on SUSE Linux_.
```
---  # apache-suse.yml

- hosts: suse

  tasks:

    - name: Install Apache
      zypper:
        name: apache2

    - name: Start Apache service
      service:
        name: apache2
        state: started

    - name: Upload index page
      copy:
        dest: /srv/www/htdocs/index.html
        src: ../ressources/index.html
        mode: 0644
...
```
