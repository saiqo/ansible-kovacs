# Cibles hétérogènes

**Le premier playbook chrony-01.yml utilisera les modules de gestion de paquets natifs apt, dnf et zypper et s'inspirera de la méthode « gros sabots » utilisée plus haut dans cet atelier.**

Voici notre playbook méthode "bourrin" pour installer chrony sur Debian, Ubuntu, Rocky et openSUSE :

``` yaml title="chrony-01.yml"
--- # chrony-01.yml
- name: Install & Setup chrony
  hosts: all

  tasks:
    # Install chrony
    - name: Update cache on Debian based system
      apt:
        update_cache: true
        cache_valid_time: 3600
      when: ansible_os_family == "Debian"

    - name: Install chrony on Debian based system
      apt:
        name: chrony
        state: present
      when: ansible_os_family == "Debian"

    - name: Install chrony on Rocky
      dnf:
        name: chrony
      when: ansible_distribution == "Rocky"

    - name: Install chrony on SUSE Linux
      zypper:
        name: chrony
      when: ansible_distribution == "openSUSE Leap"

    # Enable & start service
    - name: Enable & start chrony service
      service:
        name: chronyd
        state: started
        enabled: true     

    # Configure chrony
    - name: Configure chrony files on Debian based system
      copy:
        dest: /etc/chrony/chrony.conf
        content: |
          # chrony.conf
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
      when: ansible_os_family == "Debian"
      notify: restart_chrony

    - name: Configure chrony files on Rocky & Open suse
      copy:
        dest: /etc/chrony.conf
        content: |
          # chrony.conf
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
      when: ansible_distribution in [ "Rocky", "openSUSE Leap"]
      notify: restart_chrony

  # Handlers
  handlers:
    - name: restart_chrony
      service:
        name: chronyd
        state: restarted

...
```

**Le deuxième playbook chrony-02.yml définira trois variables chrony_package, chrony_service et chrony_confdir et utilisera le module de gestion de paquets générique package.**

Notre playbook utilise des variables définis dans le playbook. Le nom du paquet et du service sont similaire, seul le répertoire de configuration est différent :

``` yaml title="chrony-02.yml"
---  # chony-02.yml

- hosts: all
  vars:
    chrony:
      chrony_package: chrony
      chrony_service: chronyd
      
      Debian:
        chrony_confdir: /etc/chrony/chrony.conf
      Ubuntu:
        chrony_confdir: /etc/chrony/chrony.conf
      Rocky:
        chrony_confdir: /etc/chrony.conf
      openSUSE Leap:
        chrony_confdir: /etc/chrony.conf

  tasks:
    - name: Update package information on Debian/Ubuntu
      apt:
        update_cache: true
        cache_valid_time: 3600
      when: ansible_os_family == "Debian"

    - name: Install chrony
      package:
        name: "{{chrony.chrony_package}}"
        state: present

    - name: Start chrony & enable it on boot
      service:
        name: "{{chrony.chrony_service}}"
        state: started
        enabled: true
    
    - name: Copy config file
      copy:
        dest: "{{chrony[ansible_distribution].chrony_confdir}}"
        content: |
          # chrony.conf
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
      notify: restart_chrony

  handlers:
    - name: restart_chrony
      service:
        name: "{{chrony.chrony_service}}"
        state: restarted
      
...
```