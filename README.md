# miniprojet_ansible_eazytraining

## Description

Ce projet Ansible installe et configure une application Web minimale en déployant un conteneur Apache HTTPD sur des hôtes Ubuntu/Debian ou CentOS.

Le playbook principal `playbook.yml` applique le rôle `webapp` à tous les hôtes définis dans l'inventaire `host.yml`.

## Architecture

- `ansible.cfg`
  - désactive la vérification des clés SSH
  - utilise `host.yml` comme inventaire
  - charge les rôles depuis `roles`
- `host.yml`
  - définit les groupes `ubuntu` et `centos` avec leurs hôtes
- `group_vars/`
  - `all.yml` : paramètres communs à tous les hôtes
  - `ubuntu.yml` : utilisateur `ubuntu`
  - `centos.yml` : utilisateur `admin`
- `host_vars/`
  - `ubuntu-server.yml` : adresse IP et mot de passe pour l’hôte Ubuntu
  - `centos-server.yml` : adresse IP et mot de passe pour l’hôte CentOS
- `roles/webapp/`
  - installe Docker et Docker Compose
  - démarre un conteneur Apache HTTPD
  - déploie une page statique `index.html` générée depuis le template

## Rôle `webapp`

Le rôle `webapp` réalise les actions suivantes :

1. inclut les tâches spécifiques à la distribution (`docker-ubuntu.yml` ou `docker-centos.yml`)
2. installe les paquets et dépendances Docker nécessaires
3. configure et démarre le service Docker
4. ajoute l’utilisateur au groupe Docker
5. installe `docker-compose`
6. crée le fichier `index.html` à partir du template `roles/webapp/templates/index.html.j2`
7. démarre un conteneur `webapp` à partir de l’image `httpd:2.4`
8. expose le port `8080` de l’hôte vers le port `80` du conteneur

## Variables principales

- `server_port` : port exposé sur la machine hôte (par défaut `8080`)
- `container_port` : port du conteneur HTTPD (par défaut `80`)
- `ansible_user` : utilisateur SSH défini dans `group_vars/ubuntu.yml` ou `group_vars/centos.yml`

## Exécution

Pour lancer le playbook :

```bash
ansible-playbook playbook.yml
```

Assurez-vous que les hôtes sont accessibles avec les informations de connexion définies dans `host_vars/`.

## Prérequis

- Ansible installé sur la machine de contrôle
- Accès SSH aux hôtes distants
- Hôtes Ubuntu/Debian ou CentOS prêts à recevoir la configuration

## Notes

- La configuration utilise `docker_container` pour démarrer Apache dans un conteneur Docker.
- Le rôle est conçu pour supporter les distributions suivantes :
  - Ubuntu / Debian
  - CentOS
- Le mot de passe SSH est stocké en clair dans `host_vars/`, ce qui est acceptable pour un mini-projet mais doit être évité en production.


## 👤 Auteur

**KODJI Kanka**  
Formation DevOps — [EazyTraining](https://eazytraining.fr)  
GitHub : [KODJIKanka](https://github.com/KODJIKanka)

---

## 📄 Licence

Ce projet est réalisé dans le cadre d'une formation DevOps avec EazyTraining.