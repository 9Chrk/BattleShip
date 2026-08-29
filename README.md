# BattleShip

![C++](https://img.shields.io/badge/C%2B%2B-20-blue)
![SQLite](https://img.shields.io/badge/SQLite-embarqu%C3%A9-003B57)
![Licence](https://img.shields.io/badge/Licence-MIT-green)

BattleShip est une implémentation en console du jeu de bataille navale, organisée autour d’un serveur TCP et de clients locaux. Deux joueurs se connectent au serveur, placent leurs flottes et jouent à tour de rôle sur deux grilles affichées côte à côte. Le serveur gère également les comptes, les amis, les invitations, l’observation des parties et leur relecture durant son exécution.

## 📸 Aperçu

![Affichage terminal d’une partie](res/Affichage%20Terminal%20Partie.png)

## Sommaire

- [Fonctionnalités](#-fonctionnalités)
- [Prérequis](#-prérequis)
- [Installation et compilation](#️-installation-et-compilation)
- [Lancer une partie locale](#-lancer-une-partie-locale)
- [Commandes en jeu](#-commandes-en-jeu)
- [Données locales](#-données-locales)
- [Structure du projet](#-structure-du-projet)
- [Documents](#-documents)
- [Auteurs](#-auteurs)
- [Licence](#-licence)

## ⚡ Fonctionnalités

- Serveur TCP écoutant sur le port `8080` et client se connectant à `127.0.0.1`.
- Inscription et connexion des utilisateurs.
- Création d’une partie, sélection d’une partie disponible et invitation d’amis comme joueur ou observateur.
- Placement des bateaux, tirs au tour par tour et affichage de sa flotte et de la flotte adverse en terminal.
- Limites de temps configurables lors de la création d’une partie : de 15 à 60 secondes par tour et de 650 à 1 200 secondes pour la partie.
- Observation d’une partie en cours et relecture des parties accessibles aux participants pendant l’exécution du serveur.
- Liste d’amis, demandes d’amis et messages privés entre amis connectés.

## 🧰 Prérequis

- Un environnement de type Unix/Linux : le code utilise notamment les sockets POSIX, `pthread`, `dl` et la commande `clear`.
- GNU Make.
- `g++-10`, utilisé explicitement par le `Makefile`, avec la prise en charge de C++20.
- Un compilateur C pour compiler SQLite inclus dans `lib/sqlite3/`.

Aucun serveur de base de données externe ni dépendance à installer n’est requis : SQLite est fourni avec le dépôt.

## ⚙️ Installation et compilation

```bash
git clone https://github.com/9Chrk/BattleShip.git
cd BattleShip
make
```

La commande produit deux exécutables à la racine du dépôt : `battleshipServer` et `battleshipClient`.

Pour supprimer les fichiers de compilation, puis également les exécutables :

```bash
make clean
make mrclean
```

## 🎮 Lancer une partie locale

1. Dans un premier terminal, démarrez le serveur :

   ```bash
   ./battleshipServer
   ```

2. Dans deux autres terminaux, lancez un client par joueur :

   ```bash
   ./battleshipClient
   ```

3. Depuis chaque client, créez un compte ou connectez-vous. Le premier joueur crée une partie puis le second la rejoint depuis le menu. Le mode `d` correspond au mode de jeu exécuté par le serveur.

Les interactions s’effectuent directement dans les terminaux des clients. Le serveur doit rester démarré avant la connexion des clients.

## ⌨️ Commandes en jeu

Les commandes suivantes sont traitées par le serveur lorsqu’elles sont saisies dans un client connecté :

| Commande | Effet |
| --- | --- |
| `/help` | Affiche les commandes disponibles. |
| `/u` | Actualise le plateau du client lorsqu’il est dans une partie. |
| `/mp ami message` | Envoie un message privé à un ami connecté. |
| `/invite ami 0` | Invite un ami à la partie créée par le joueur comme participant. |
| `/invite ami 1` | Invite un ami à la partie créée par le joueur comme observateur. |

Les actions de messagerie et d’invitation nécessitent que les deux utilisateurs soient amis et connectés.

## 🗃️ Données locales

Au démarrage, le serveur ouvre la base SQLite `src/server/database.sqlite` et crée la table des utilisateurs si elle n’existe pas. Les comptes, mots de passe, relations d’amitié et demandes d’amis sont donc conservés dans ce fichier local.

Les relectures sont conservées en mémoire par le serveur : elles sont disponibles pendant son exécution.

## 🧱 Structure du projet

```text
.
├── src/
│   ├── client/             # Connexion locale, réception des événements et affichage terminal
│   ├── common/             # Port réseau et format des messages partagés
│   └── server/             # Parties, grilles, menus, comptes et serveur TCP
├── lib/sqlite3/            # Sources SQLite compilées avec le serveur
├── res/                    # Capture de l’affichage terminal
├── diagramme/              # Diagrammes client, serveur et cas d’utilisation
├── Makefile                # Compilation des exécutables client et serveur
├── Consignes.pdf           # Consignes du projet
├── Enoncé.pdf              # Énoncé du projet
└── SRD.pdf                 # Document SRD
```

## 📄 Documents

- [Consignes](Consignes.pdf)
- [Énoncé](Enoncé.pdf)
- [SRD](SRD.pdf)

## 👥 Auteurs

- Othman El Kazbani — 493194
- FatimaZohra Lahrach — 536142
- Jawad Cherkaoui — 576517

## 📜 Licence

Ce projet est distribué sous licence [MIT](LICENSE).
