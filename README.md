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
