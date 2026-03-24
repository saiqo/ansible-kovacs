---
icon: material/note-edit
---

# Commandes utiles

Listes de commandes utiles

## Vagrant
```
vagrant box list
vagrant up <image>
vagrant destroy -f <image> | vagrant destroy -f
vagrant ssh <vm>
vagrant global-status
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

## Debian/Ubuntu
Pour effectuer une recherche dans le cache uniquement sur le nom des paquets
``` bash
apt-ache search --names-only ansible
```

