<div align="center">
<h1>Mémo d'utilisation de git (seul ou en équipe)</h1>

<div>
    <a href="https://github.com/lbAntoine" target="_blank">Mon profil</a>
•
    <a href="https://open.spotify.com/playlist/3o0OqYN0EFmReWTdlbybAW?si=D9RAH_usT9yd8Dmdj7n-Qg" target="_blank">Ma playlist</a>
•
    <a href="https://www.buymeacoffee.com/lbAntoine" target="_blank">Me soutenir</a>
</div>
</div>

**Préambule** :

Bien que plusieurs solutions graphiques existent pour la gestion de projet git, ce mémo est basé sur le CLI (Command line interface). La raison principale étant que les entreprises n'utilisent pas souvent les interfaces graphiques ou bien si c'est le cas ce ne sont pas souvent les mêmes. Les compétences d'utilisation de git via le CLI sont, quant à elle, immuables.

## Introduction

Que ce soit sur [GitHub](https://github.com/), [BitBucket](https://bitbucket.org/) ou [GitLab](https://gitlab.com/gitlab-org/gitlab) (à la préférence de chacun), l'utilisation de git est la même. Chaque développeur a une copie complète du projet avec tout son historique, ce qui permet de travailler en parallèle sans dépendre d'une connexion réseau constante. Les modifications sont enregistrées sous forme de _commits_, chaque commit représentant un état spécifique du projet à un moment donné. On peut ensuite fusionner les changements de différentes branches, résoudre les conflits et revenir à des versions antérieures si nécessaire. Grâce à Git, la collaboration et la gestion des versions deviennent plus efficaces et sécurisées. [GitHub](https://github.com/), [BitBucket](https://bitbucket.org/) et [GitLab](https://gitlab.com/gitlab-org/gitlab) sont _simplement_ des hébergeurs cloud pour vos projets (ils ont aussi des fonctionnalités aidant à la gestion du code source d'un projet).

Ces solutions sont toutes présentées de la même manière. Pour commencer, il faut [avoir un compte](https://github.com/signup?ref_cta=Sign+up&ref_loc=header+logged+out&ref_page=%2F&source=header-home) et s'y [connecter](https://github.com/login). Tu peux prendre le temps de naviguer sur le site pour te familiariser avec les menus.

Avant toute chose, assurons nous que ton ordinateur est muni d'une clé qui te permettra de communiquer avec ton compte sur la solution que tu as choisi (pour les exemples et illustrations, j'ai choisi [GitHub](https://github.com/), mais tout ce que tu pourras trouver ici est reproduisible sur les autres solutions).

Un des protocols que peut utiliser Git est le [SSH](https://fr.wikipedia.org/wiki/Secure_Shell). Pour l'utiliser il faut que tu génère une clé (sauf si tu en as déjà une, ou bien que tu en veuilles une spécifiquement pour cette utilisation - ce que je recommande). Il existe plusieurs algorithmes de chiffrement des clés mais pour celle que nous allons utiliser avec le service de ton choix, nous suivrons le [tutoriel de GitHub](https://docs.github.com/fr/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) qui recommande l'utilisation de `Ed25519`.

```bash
ssh-keygen -t ed25519 -C "your.email@example.com"
```

L'ajout de mot de passe est à ta discrection (je te le conseille tout de même, et dans le [partie 5](#5-gh-cli-spécificités-github) je te montre comment éviter d'avoir à le retaper tout le temps).

Une fois générée, tu pourras trouver deux versions de la clé dans le dossier `.ssh` qui se trouve à la root de ton utilisateur (`~/.ssh` pour linux ou mac, `C:\Users\TonUtilisateur\.ssh` sur windows). Si tu n'as pas précisé de nouveau nom, la nouvelle clé portera le nom de l'algorithme. Il y aura aussi une version `.pub`, c'est celle ci qui nous intéresse. Ouvre la avec un éditeur de texte ou simplement avec la commande `cat ed25519.pub` et copies son contenu (Cette étape peut être améliorée avec l'utilisation du CLI GH si tu utilises GitHub - voir [partie 5](#5-gh-cli-spécificités-github)). Ensuite dans les paramètres de compte de ton compte, tu devrais trouver une option clés SSH/GPG. Tu pourras ajouter ta clé ici.

<div align="center">
<img align="center" alt="sshkey" src="https://utfs.io/f/f8c51a88-7820-427c-8bc0-4b2fd644d3d6-feeo3u.png" />
<br>

_ajouter une clé SSH_

</div>

Assurons nous d'une chose aussi... tu as bien installé git sur ton ordinateur ? Un petit coup de `git --version` dans ton terminal ne fera pas de mal. Si la commande n'est pas trouvée alors il faut l'installer. Sinon le terminal devrait te répondre comme ça :

```bash
git version 2.34.1 # Ta version peut différer
```

### Installer git

#### Unix systems (Linux, macOS...)

Avec linux, simplement utiliser le package manager de la distribution utilisée :

```bash
sudo apt install git # Ubuntu
sudo dnf install git # Fedora
sudo pacman -S git # Arch linux
sudo xbps-install git # Void
```

Avec macOS, il faut avoir le package manager [HomeBrew](https://brew.sh/) d'installer puis exécuter la commande suivante :

```bash
brew install git
```

#### Windows

Pour windows c'est légèrement différent. Télécharge [l'installer](https://git-scm.com/download/win) ou bien utilises la commande [winget](https://github.com/microsoft/winget-cli) dans ton powershell (si tu as winget d'installé) :

```powershell
winget install --id Git.Git -e --source winget
```

---

## In the next episode...

Dans la [partie suivante](./lesbasesdegit.md) nous parlerons de la base de git, la création d'un repository, le fichier _.gitignore_ et la bases des branches.

Si tu aimes ce contenu et que tu le trouves utile, n'hésite pas à le mettre dans tes favoris et à suivre [mon profil github](https://github.com/lbAntoine).

<div align="center">
<div>
<a href="/">Accueil</a>
•
<a href="./lesbasesdegit.html">Partie suivante »</a>
</div>
<img width="30%" src="https://utfs.io/f/35969b6d-f22c-4a41-9775-a54026f1ff73-mwy9q0.png" />
<div>
<a href="https://discordapp.com/users/328163554991669251" target="_blank">Discord</a>
•
<a href="https://linkedin.com/in/antoine-le-bras/" target="_blank">LinkedIn</a>
</div>
</div>
