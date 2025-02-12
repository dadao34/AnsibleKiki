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

