# Atelier 01 - Installation d'Ansible

### Challenge n°1 - Installation sur Ubuntu
Ubuntu étant basé sur debian, nous pouvons lancer les mêmes commandes que pour debian.

Démarrez la VM ubuntu depuis le répertoire atelier-01 : 
``` bash
$ vagrant up ubuntu
```

Connectez-vous à cette VM :
``` bash
$ vagrant ssh ubuntu
```

Rafraîchissez les informations sur les paquets :
``` bash
$ sudo apt update
```

Recherchez le paquet ansible avec les options qui vont bien :
``` bash
$ apt-cache search --names-only ansible
```
![alt text](image3.png)

Installez le paquet officiel fourni par la distribution :
``` bash
$ sudo apt install -y ansible
```

Vérifiez si l'installation s'est bien déroulée :
``` bash
$ ansible --version
```

Notez la version d'Ansible :
![alt text](image4.png)

Notre ansible est en version `2.10.8`

Déconnectez-vous et supprimez la VM :
``` bash
$ exit
$ vagrant destroy -f ubuntu
```

### Challenge n°2 - Installation sur Ubuntu avec repo PPA
Préparation de la VM :
``` bash
$ vagrant up ubuntu
$ vagrant ssh ubuntu
```

On ajoute ensuite le dépôt PPA pour ansible :
``` bash
$ sudo apt-add-repository ppa:ansible/ansible
```

Puis on installe ansible :
``` bash
$ sudo apt update
$ sudo apt install -y ansible
```

On vérifie notre version :
``` bash
$ ansible --version
```
![alt text](image5.png)

Notre ansible est en version `2.17.14`, donc une version bien plus récente que celle proposé par les dépots ubuntu/debian  

### Challenge n°3 - Installation sur Rocky avec pip
Lancement de la VM Rocky :
``` bash
$ vagrant up rocky
$ vagrant ssh rocky
```

Installation de PIP :
``` bash
$ sudo dnf install -y python3-pip
```

Création du venv :
``` bash
$ python3 -m venv ~/.venv/ansible
```

``` bash
$ source ~/.venv/ansible/bin/activate
(ansible) $
```

Mise à jour de pip :
``` bash
(ansible) $ pip install --upgrade pip
```

Installation d'ansible :
``` bash
(ansible) $ pip install ansible
```

Vérification de la version d'ansible
``` bash
(ansible) $ ansible --version
```
![alt text](image6.png)

On a donc un ansible en version `2.15.13`

Pour terminer, on fait du ménage : 
``` bash
(ansible) $ deactivate
$ exit
$ vagrant destroy -f rocky
```