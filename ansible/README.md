# Ansible


**But** : depuis la machine A (control node) lancer une seule commande pour faire `apt update && apt upgrade -y && apt autoremove -y` sur Machine A et B.

## Contenu
- `inventory.ini` : inventaire Ansible (remplace `USER_REPLACE` par ton utilisateur, ex: `ubuntu` ou ``saucisse``)
- `ansible.cfg`  : config minimale (remplace `USER_REPLACE`)
- `playbook.yml` : playbook qui exécute les actions apt
- `update_all.sh` : script wrapper pour lancer le playbook

## Prérequis (sur Machine A)
```bash
sudo apt update
sudo apt install -y ansible sshpass
```
- Les deux VM (A et B) doivent être sur le même réseau (Host-Only ou Internal Network).
- Remplace `USER_REPLACE` dans `inventory.ini` et `ansible.cfg` par ton nom d'utilisateur sur les VM (ex: `ubuntu`).

## SSH sans mot de passe (recommandé)
Sur A :
```bash
ssh-keygen -t ed25519
ssh-copy-id USER_REPLACE@192.168.56.102
```
Vérifie :
```bash
ssh USER_REPLACE@192.168.56.102 hostname
```

## Utilisation
1. Éditer `inventory.ini` et `ansible.cfg` : remplacer `USER_REPLACE` et l'adresse IP
2. Rendre le script exécutable :
   ```bash
   chmod +x update_all.sh
   ```
3. Lancer le playbook :
   ```bash
   ./update_all.sh
   # ou
   ansible-playbook playbook.yml -v
   ```
- Si les comptes sudo demandent un mot de passe, ajoute `--ask-become-pass` quand tu lances `ansible-playbook`.

## Remarque sur la sécurité
- Dans un lab, évitez d'utiliser `host_key_checking = False` en prod. Ici c'est pour faciliter le TP
