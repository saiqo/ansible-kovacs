---
icon: material/note-edit
---

# Commandes utiles

Listes de commandes utiles

## Vagrant
```
vagrant box list
vagrant up <image>/<vm>
vagrant destroy -f <image> | vagrant destroy -f
vagrant ssh <vm>
vagrant global-status
vagrant halt <vm>
```

## Ansible
Ping (module ansible) toutes les machines
``` bash
ansible -m ping all
```

Afficher la liste des hôtes de notre inventaire
``` bash
ansible all --list-hosts
```

Ansible config
``` bash
ansible-config view # Affiche la config actuelle
ansible-config dump # Affiche toutes les options possible pour la config

```

Choisir le nombre de fork (nombre de connexions en parallèles)
```
ansible-playbook hello-all.yml -f 1
```

Pour lancer à partir d'une tâche dans un playbook
``` bash
ansible-playbook apache-01.yml --start-at-task="*page*"
```

## Debian/Ubuntu
Pour effectuer une recherche dans le cache uniquement sur le nom des paquets
``` bash
apt-ache search --names-only ansible
```

