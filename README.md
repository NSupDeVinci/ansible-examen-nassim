# Déploiement WordPress – Rôle Ansible multi-OS (Debian/Rocky)

**Projet réalisé dans le cadre d’un exercice d’automatisation de déploiement WordPress avec Ansible.**
Sources de l’exercice :

* [README de l’évaluation](https://gitlab.com/ftutorials-evaluations/ansible/-/blob/main/evaluation-1/README.md)
* [Script Bash de base à convertir](https://gitlab.com/ftutorials-evaluations/ansible/-/blob/main/evaluation-1/Install_wordpress.sh)

## Présentation

Ce rôle Ansible automatise l’installation complète d’un site **WordPress** avec **Apache** et **MariaDB**,
pour des serveurs **Debian** et **Rocky Linux** (RedHat/CentOS/AlmaLinux).

Le rôle :

* Installe les paquets nécessaires selon l’OS (compatibilité Debian/RedHat),
* Télécharge et déploie WordPress,
* Prépare la base de données et l’utilisateur MariaDB,
* Configure Apache pour WordPress (template adaptable),
* Gère les permissions et l’activation des sites/modules Apache.

---

## Structure du rôle

```
ansible-examen-nassim/
├── defaults/
│   └── main.yml
├── handlers/
│   └── main.yml
├── meta/
│   └── main.yml
├── tasks/
│   └── main.yml
├── templates/
│   └── wordpress.conf.j2
├── tests/
│   ├── inventory
│   └── test.yml
├── vars/
│   └── main.yml
└── README.md
```

---

## Variables personnalisables

* `defaults/main.yml` :

  ```yaml
  wordpress_db_name: wordpress
  wordpress_db_user: wordpress
  wordpress_db_password: wordpress
  wordpress_db_host: localhost
  wordpress_download_url: https://wordpress.org/latest.tar.gz
  wordpress_dir: /var/www/html/wordpress
  apache_site_conf: /etc/apache2/sites-available/wordpress.conf
  ```
* `vars/main.yml` :
  Listes de paquets spécifiques à chaque distribution (Debian/Rocky),
  chemins de configuration Apache, noms de services.

---

## Prérequis

* Ansible ≥ 2.9
* Un ou plusieurs hôtes Linux Debian 10/11/12 ou Rocky Linux 8/9 (inventaire à renseigner dans `tests/inventory`)
* Accès sudo/root sur les hôtes
* Les modules Ansible [community.mysql](https://docs.ansible.com/ansible/latest/collections/community/mysql/) (installés par défaut sur la plupart des environnements, sinon : `ansible-galaxy collection install community.mysql`)

---

## Utilisation

### 1. Préparer l’inventaire

Dans `tests/inventory` :

```
[debian]
debian1 ansible_host=192.168.56.101 ansible_user=vagrant

[rocky]
rocky1 ansible_host=192.168.56.102 ansible_user=vagrant
```

Adapte les adresses IP/utilisateurs selon ton environnement.

### 2. Lancer le rôle via un playbook de test

Exemple : `tests/test.yml` :

```yaml
---
- name: Déploiement WordPress
  hosts: all
  become: true
  roles:
    - ansible-examen-nassim
```

Lance la commande depuis la racine du projet :

```bash
ansible-playbook -i tests/inventory tests/test.yml
```

### 3. Accéder à WordPress

* **Sur Debian** :
  Ouvre : [http://\<ip\_debian>/wordpress/](http://<ip_debian>/wordpress/)
* **Sur Rocky** :
  Ouvre : [http://\<ip\_rocky>/wordpress/](http://<ip_rocky>/wordpress/)

Si tu utilises Vagrant ou des VMs en NAT, pense à rediriger le port 80 si besoin.

---

## Personnalisation

* **Variables** : personnalise le nom de la base, l’utilisateur ou le mot de passe dans `defaults/main.yml`.
* **Configuration Apache** : adapte le template dans `templates/wordpress.conf.j2` si besoin.
* **Ajout d’extensions PHP** : complète la liste dans `vars/main.yml`.

---

## Notes pédagogiques

Ce projet est issu de l’exercice dont les consignes et ressources sont consultables ici :

* [Énoncé README](https://gitlab.com/ftutorials-evaluations/ansible/-/blob/main/evaluation-1/README.md)
* [Script Bash d’origine](https://gitlab.com/ftutorials-evaluations/ansible/-/blob/main/evaluation-1/Install_wordpress.sh)

Il s’agit d’une traduction fidèle du script bash en un rôle Ansible **multi-distributions**, avec gestion propre des tâches, handlers, variables et templates.

---

**Pour toute amélioration, personnalisation ou question sur la compatibilité, consulte le code ou contacte l’auteur du rôle.**
