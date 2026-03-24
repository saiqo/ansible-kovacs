---
icon: material/note-edit
---

# Commandes utiles

## Vagrant
```
vagrant box list
vagrant up <image>
vagrant destroy -f <image> | vagrant destroy -f
vagrant ssh <vm>
vagrant global-status
```

## Ansible
```bash
ansible testing --list-hosts #affiche toutes les machines qui sont compris dans le groupe "all"
```

## Debian/Ubuntu
Pour effectuer une recherche dans le cache uniquement sur le nom des paquets
``` bash
apt-ache search --names-only ansible
```

