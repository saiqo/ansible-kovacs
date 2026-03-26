# Facts et variables implicites


**Écrivez trois playbooks pour afficher des informations sur chacun des Target Hosts :**

* pkg-info.yml pour afficher le gestionnaire de paquets utilisé
```yml
---  # pkg-info.yml

- hosts: all

  tasks:

    - name: Affiche gestionnaire de paquet
      debug:
        msg: "La machine {{ inventory_hostname }} utilise {{ ansible_pkg_mgr }} en gestionnaire de paquet."

```
Resultat:

![alt text](image.png)

* python-info.yml pour afficher la version de Python installée
```yml

```
* dns-info.yml pour afficher le(s) serveur(s) DNS utilisé(s)
```yml

```