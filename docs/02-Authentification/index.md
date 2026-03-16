# Atelier 03 - Authentification

On se place dans le bon dossier : 
``` bash
$ cd ~/formation-ansible/atelier-03
```

Puis on lance les VM avec :
``` bash
$ vagrant up
```

On se connecte a la vm control :
``` bash
$ vagrant ssh control
```

**Pour réussir un ping Ansible, il faut faire quelques étapes au préalable :**


On modifie notre fichier `/etc/hosts` de cette façon :
``` bash
# /etc/hosts
127.0.0.1      localhost.localdomain  localhost
192.168.56.10  control.sandbox.lan    control
192.168.56.20  target01.sandbox.lan   target01
192.168.56.30  target02.sandbox.lan   target02
192.168.56.40  target03.sandbox.lan   target03
```

On peut vérifier avec un ping si la résolution DNS fonctionne :
``` bash
$ for HOST in target01 target02 target03; do ping -c 1 -q $HOST; done
```
![alt text](image.png)

On récupère la clé publique de nos différentes VMs :
``` bash
$ ssh-keyscan -t rsa target01 target02 target03 >> ~/.ssh/known_hosts
```
![alt text](image-1.png)

On génère ensuite une paire de clé sur notre VM control :
``` bash
$ ssh-keygen
```

On envoie notre clé publique sur les différentes VMs targets :
``` bash
$ ssh-copy-id vagrant@target01
$ ssh-copy-id vagrant@target02
$ ssh-copy-id vagrant@target03
```

On vérifie que nous pouvons faire un ping ansible :
``` bash
$ ansible all -i target01,target02,target03 -m ping
```
![alt text](image-2.png)

Tout fonctionne !

