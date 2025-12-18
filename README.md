# Ansible Role: CIS Apache HTTP Server 2.4 Benchmark

![Ansible](https://img.shields.io/badge/ansible-role-blue?style=flat-square)
![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)

Ce projet contient un **rôle Ansible** complet pour appliquer les recommandations de sécurité du **CIS Apache HTTP Server 2.4 Benchmark v2.3.0**.

Il a été généré automatiquement avec l'aide d'une IA Agentique (Google DeepMind) après analyse du document PDF officiel du CIS.

## Fonctionnalités

Le rôle `cis_apache` couvre les 11 sections du benchmark CIS :

- **Installation et Planification** : Vérification des paquets et utilisateurs.
- **Minimisation des Modules** : Désactivation automatique des modules inutiles (WebDAV, Proxy, Autoindex, etc.).
- **Permissions et Propriété** : Sécurisation stricte des fichiers de configuration et des répertoires.
- **Contrôle d'Accès** : Blocage de l'accès à la racine `/`, désactivation de `.htaccess` par défaut.
- **Contenu et Options** : Suppression des pages par défaut, scripts CGI, et méthodes HTTP dangereuses (TRACE).
- **Logging et Audit** : Configuration des logs et intégration de **ModSecurity** (OWASP CRS).
- **SSL/TLS** : Désactivation des vieux protocoles (TLS 1.0/1.1), ciphers faibles, et activation HSTS.
- **Fuite d'Information** : `ServerTokens Prod`, `ServerSignature Off`, etc.
- **Déni de Service (DoS)** : Configuration des Timeouts et Limites de requêtes.
- **SELinux** : Application du mode Enforcing.

## Pré-requis

- Ansible 2.9+
- Serveur cible sous RHEL/CentOS 8+ ou Debian/Ubuntu.
- Accès root (become).

## Installation

Clonez ce dépôt dans votre répertoire de projets Ansible :

```bash
git clone https://github.com/Tiherisson/cis_security_project.git
cd cis_security_project
```

## Utilisation

Un playbook d'exemple `cis_apache_hardening.yml` est fourni pour appliquer le rôle.

### 1. Configurer l'inventaire
Assurez-vous que vos hôtes sont définis dans votre fichier `hosts` (groupe `webservers` par défaut).

### 2. Exécuter le playbook

**Mode Check (Simulation) :**
```bash
ansible-playbook cis_apache_hardening.yml --check
```

**Application réelle :**
```bash
ansible-playbook cis_apache_hardening.yml
```

## Variables

Toutes les variables sont définies dans `roles/cis_apache/defaults/main.yml`. Vous pouvez les surcharger selon vos besoins.

| Variable | Défaut | Description |
|----------|--------|-------------|
| `cis_apache_install_packages` | `true` | Installe Apache si absent |
| `cis_apache_listen_ip` | `0.0.0.0` | IP d'écoute (Doit être spécifique pour CIS) |
| `cis_apache_enable_mod_security` | `true` | Installe ModSecurity |
| `cis_apache_server_tokens` | `Prod` | Valeur de ServerTokens |
| `cis_apache_ssl_enabled` | `true` | Active les configurations SSL |

*(Voir `defaults/main.yml` pour la liste complète)*

## Structure du Projet

```
.
├── cis_apache_hardening.yml  # Playbook principal
├── roles/
│   └── cis_apache/           # Rôle de sécurisation
│       ├── defaults/         # Variables par défaut
│       ├── tasks/            # Tâches divisées par section CIS
│       └── handlers/         # Gestionnaires (redémarrage service)
└── CIS_Benchmark.txt         # Extrait du document de référence
```

## License

MIT
