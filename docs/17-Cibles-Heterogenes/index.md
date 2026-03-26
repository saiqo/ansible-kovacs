# Cibles hétérogènes

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

```