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

## 1. Bases de git

**Sommaire**

- **[1.1. Création d'un repository et ajout d'un remote pour un nouveau projet](#11-création-dun-repository-et-ajout-dun-remote-pour-un-nouveau-projet)**
- **[1.2. .gitignore](#12-gitignore)**
- **[1.3. Les branches, travailler en équipe](#13-les-branches-travailler-en-équipe)**

---

## 1.1. Création d'un repository et ajout d'un remote pour un nouveau projet

> Pour créer un _repository_ (repo dans la suite) il existe plusieurs procédures dépendent de la manière dont un projet est initialisé. Nous partirons du principe que le projet a été initialisé en local avant la création du repo.

Sur ton ordinateur, tu as un dossier contenant un projet que tu veux versionner et rendre disponible à ton équipe sur GitHub. Pour l'exemple, le projet ici ressemble à ça :

<div align="center">
<img align="center" alt="sshkey" src="https://utfs.io/f/3c2aea44-97ab-4c3d-8409-2972ebee9494-qimi81.png" />
<br>

_projet exemple sur l'ordinateur local_

</div>

Avant de nous intéresser aux commandes que nous devrons exécuter, nous allons d'abord s'assurer de créer le repository en ligne pour accueillir notre projet (sa maison). Pour ce faire, depuis n'importe quelle page sur Github tu peux cliquer sur le petit plus dans la barre de navigation en haut et sélectionner _New repository_.

<div align="center">
<img align="center" alt="sshkey" src="https://utfs.io/f/8d58c846-b1aa-409d-a80d-ebaea70054d6-hcg077.png" />
<br>

_création d'un nouveau repo_

</div>

> NB : Si le repo existe déjà il suffit d'exécuter la commande `git clone git@github.com:nomducompte/nomduprojet.git`

Une fois le repo créé, tu peux récupérer le lien SSH (nous avons mis en place la connection avec une clé SSH alors ne prenons pas le lien HTTP). Le lien devrait ressembler à celui-ci `git@github.com:nomdecompte/nomdeprojet.git`.

Retournons dans le dossier de notre projet, dans notre terminal. Avant toute chose, les moeurs changent et git a parfois du mal à s'y mettre, nous allons changer la branche initiale par défaut dans la configuration de git avec la commande `git config --global init.defaultBranch main` afin de passer la branche par défaut de _master_ à _main_.

Nous allons donc initialiser le projet en tant que repo git local avec la commande `git init`. Cette commande va créé un dosier _.git_ qui contiendra toutes les informations liées au repo (les changements, les branches, les versions etc...). Pour l'instant tous les fichiers sont considérés comme nouveau. A noter que dans ce projet exemple aucun dossier ni fichier ne contient de fichiers que nous ne désirons pas pousser dans le repo origin (celui hébergé par GitHub), dans le [chapitre suivant](#12-gitignore) nous verrons comment éviter d'envoyer certains fichiers.

Nous allons effectuer deux commandes avant de connecter notre repo local au repo remote. Tout d'abord `git add .` va ajouter tous les fichiers nouveau à notre commit ([dans le cheatsheet](./cheatsheetgit.md) tu peux retrouver une description plus ample de la commande). Puis `git commit -m 'init'` qui va valider le commit actuel en y attanchant le message "init".

Finalement nous allons connecter notre repo local au repo distant (oui origin, puis remote, puis distant... accordons nous à dire remote pour le reste). La commande qui le permet est la suivante : `git remote add origin git@github.com:nomdecompte/nomdeprojet.git`. Maintenant nous pouvons pousser (du mot anglais push que nous allons utiliser comme tel dans la suite) notre commit dans le repo remote avec la commande `git push -u origin main`. Le flag `-u` est un raccourci pour `--set-upstream` qui permet de dire à git d'attacher la branche actuelle (main) à une nouvelle branch main créée sur le repo remote. Ce flag n'est plus nécessaire après avoir push sur une branche pour une première fois (ou bien que la branche est été pull du remote directement).

Et voilà ! Si nous retournons sur la page du projet nous devrions voir nos fichiers tout fraîchement arrivés sur notre repo remote, accompagné du message "init".

<div align="center">
<img align="center" alt="sshkey" src="https://utfs.io/f/d109cdc1-eda6-4c54-996a-1111e8240e1a-27p96y.png" />
<br>

_repo remote après le premier push_

</div>

Récapitulatif des commandes :

```bash
# Configurer git pour que la branche par defaut s'appelle main et non master
git config --global init.defaultBranch main
# Initialiser le repo local
git init
# Ajouter les fichier du projet
git add .
# Valider le commit avec un message
git commit -m 'init'
# Connecter le repo local avec le repo remote
git remote add origin git@github.com:lbAntoine/example-project.git
# Push le projet pour la première fois
git push -u origin main
```

## 1.2. .gitignore

Avant de s'attaquer à des concepts un peu plus poussés, il est important de s'habituer aux bonnes pratiques. Admettons que nous avons un projet un peu plus gros et avec des fichiers qui contiennent des données sensibles, ou bien des dossiers contenant les librairies qu'utilise notre projet. Ces dossier sont gros et contiennent une trop large quantité de contenu. De plus les librairies utilisées sont souvent contenues dans un fichier à la base de notre projet (_package.json_ pour des projets nodejs, _Cargo.toml_ pour des projets rust par exemple). Alors il est contre-productif de les push dans notre repo remote (pour les fichiers contenant des données sensibles, je n'ai pas besoin de justifier pourquoi il ne faut pas les push sur le repo remote, il faudra par contre les communiquer à ton équipe par d'autres moyens).

Pour éviter d'envoyer ces fichiers et dossiers, nous pouvons créer un fichier à la racine de notre projet appelé `.gitignore`. Techniquement nous devrions mettre en place ce fichier avant même de faire notre premier push, mais dans la logique de ce mémo, c'est plus simple de comprendre à quoi sert ce fichier après avoir compris la base de git (la partie sauvegarde de projet).

Il est bon de noter que maintenant beaucoup de templates de projets (qui peuvent être installés par des frameworks) incluent ce fichier de base avec une configuration pré-remplie. Mais il est tout de même intéressant de comprendre son fonctionnement pour éventuellement y faire des modifications toi même par la suite.

J'ai ajouté une librairie dans notre projet exemple avec nodejs, ce qui a créé un dossier _node_modules_ à la racine du projet. Si nous exécutons la commande `git status` nous pouvons voir ce que git comprend de l'état de nos fichiers dans notre projet et nous pouvons observer que ce dossier _node_modules_ est considéré comme une nouveauté.

<div align="center">
<img align="center" alt="sshkey" src="https://utfs.io/f/b5740a4e-321e-4b6e-9a42-edf74caccccf-iy378h.png" />
<br>

_dossier node_modules non ignoré_

</div>

C'est la que le fichier `.gitignore` entre en jeu. Ce fichier va faire comprendre à git qu'il ne doit pas prendre en compte la liste de dossier et/ou fichier nommés à l'interieur de celui-ci quand il fait son état des lieux de notre projet local par rapport à l'état en remote. Ces fichiers ne seront donc ni affecté par un _push_ ni par un _pull_. Nous allons créer ce fichier et y ajouter le nom des dossiers et fichiers que nous voulons exclure du repo remote.

```gitignore
node_modules
```

Maintenant si nous exécutons la commande `git status` une nouvelle fois, nous pouvons nous appercevoir que le dossier _node_modules_ a disparu des nouveautés vues par git.

<div align="center">
<img align="center" alt="sshkey" src="https://utfs.io/f/0984e130-8a94-4306-bc65-d15dc0b78f8c-szcceq.png" />
<br>

_dossier node_modules ignoré_

</div>

Parlons de l'output de la commande `git status` un moment. Cette commande nous permet de comprendre un peu plus comment git fonctionne et comprend l'état de notre projet. En l'état actuel (de la dernière capture d'écrant), git compred qu'un fichier déjà existant dans le repo remote a été modifié (le fichier `package.json`), et aussi que deux nouveaux fichiers ont fait leur apparition (`.gitignore` et `pnpm-lock.yaml`). Avec les indices donnés par git, nous comprenons que nous pouvons sélectionner parmis les changements et/ou les nouveautés ce que nous voulons inclure dans un commit que nous pusherons par la suite sur le repo remote.

Dans notre cas nous voulons apporter tous les changements au remote alors nous répeterons les commandes vues dans le [chapitre précédent](#11-création-dun-repository-et-ajout-dun-remote-pour-un-nouveau-projet).

```bash
# Ajouter les nouveau fichiers et les modifications au commit
git add . # Ou git add -a
# Valider le commit avec un message
git commit -m 'ajout ignore'
# Push au repo remote
git push
```

Rappel ! Comme indiqué plus tôt, maintenant que la branche main a été liée à la branche main du repo remote, il n'y a plus besoin d'utiliser le flag `-u`.

Maintenant nous pouvons aller voir la page de notre repo remote pour se rendre compte que le nouveau commit est bien arrivé et que le dossier _node_modules_ non voulu a bien été ignoré.

<div align="center">
<img align="center" alt="sshkey" src="https://utfs.io/f/cb39f892-44cd-4d67-8281-321e6651f21e-s4cmpd.png" />
<br>

_repo remote après le deuxième push_

</div>


## 1.3. Les branches, travailler en équipe

Les branches sont un concept coeur de git lorsqu'il s'agit du travail en équipe. Les branches permettent à des développeurs de travailler simultanément sans causer de problèmes de version ni de sauvegarde.

Souviens toi, il faut garder en tête le concept du repo local et du repo remote. Idéalement le repo local est celui sur lequel le développeur travaille et sur lequel il fait ses modifications. Le repo remote, lui, est une sauvegarde de l'état du projet à un moment donné. Eh bien chaque branche peut être considéré comme une sauvegarde alternative qui débute à un moment précis de l'état d'une autre branche (la branche par défaut "main" est une branche).

<div align="center">
<img align="center" alt="sshkey" src="https://utfs.io/f/61487086-da9c-452e-8aa5-783cc6f919ed-b8c9yz.png" />
<br>

_illustration branches git_

</div>

Jusqu'à maintenant, dans l'exemple, nous étions le seul développeur. Pour expliquer comment le système de branche fonctionne, nous allons l'utiliser pour simuler un ou plusieurs autres développeurs sur le même projet.

Pour ce faire, admettons d'abord que la branche main sera la branche de production, celle sur laquelle nous n'apportons pas de modification directe. Nous allons créer deux branches - a et b -. Il existe deux commandes pour créer une branche, nous allons utiliser les deux pour créer chacune des branches.

```bash
git branch a # créé la branche a
git checkout a # place le local sur la branche a
git push -u origin a # Pour que le repo remote ait aussi la branche a
# Revenons sur la branche main
git checkout main
# Créons la branche b avec une commande raccourcie
git checkout -b b # créé la branche b et checkout branch b
```

Maintenant notre repo d'exemple ressemble à peu près à ça :

<div align="center">
<img align="center" alt="sshkey" src="https://utfs.io/f/b423555e-163c-4e1b-9e2b-02493ff5d77b-f4jtv.png" />
<br>

_illustration branches git_

</div>

Comme les deux nouvelles branches (a et b) ont été créée à partir du dernier commit de main, les trois branches sont au même niveau. Pour la représentation dans l'illustration elle sont séparées pour la clarté mais la liaison permet de montrer qu'elle sont au même niveau.

Mettons en place le jeu de rôle. Nous sommes le développeur A, et notre collaborateur est le développeur B. Imaginons que notre modification soit un peu plus longue que celle de notre collaborateur et ait aussi besoin d'une modification venant de la branche de notre collaborateur.

```bash
# Copie colle les commandes suivantes, c'est pour simuler le travail du collaborateur
git checkout b
touch src/fichierB.js
echo "export const valeurImportante = 15;" >> src/fichierB.js
git add . && git commit -m 'branch b push'
git push
git checkout a
```

Sur notre branche (a) nous allons ajouter un fichier `fichierA.js` dans le dossier `src` et un fichier `index.js` à la racine du projet. Dans le fichier `fichierA.js` nous allons écrire la fonction suivante.

```javascript
export function showNstring(n) {
    const sampleSentence = "Bonjour je suis une string exemplaire.";
    return sampleSentence.slice(n).trim();
}
```

Et dans le fichier main, ajoutons la fonction suivante.

```javascript
import { showNstring } from "./src/fichierA.js";

const result = showNstring(15);
console.log("Résultat : ", result);
```

Si maintenant nous exécutons ce code avec la commande `node ./index.js`, nous devrions avoir l'output suivant : `une string exemplaire` (il se peut que tu aies une erreur, dans ce cas ajoute cette ligne dans ton fichier `package.json` : `"type": "module"`).

Maintenant admettons que le travaille de notre collaborateur était de mettre en place des variables globales pour le projet. Ainsi, au lieu de donner le paramètre `15` dans l'appelle de fonction dans index, nous voudrions importer la variable globale.

Tout d'abord, pushons nos modifications sur notre branche remote (te souviens tu des commandes ?) `git add . && git commit -m 'Push branch a' && git push`.

Voici l'état du repo après notre dernier push.

<div align="center">
<img align="center" alt="sshkey" src="https://utfs.io/f/4590fc1e-6add-4578-9b59-f1114e9a9ec4-f4jtu.png" />
<br>

_illustration branches git_

</div>

Pour récupérer les variables créées par notre collaborateur nous allons devoir effectuer un merge de sa branche dans la notre. Pour ce faire la procédure est la suivante :

```bash
# Récupérer toutes les informations du repo remote
git fetch origin
# Se rendre sur la branche b pour récupérer les
# dernières modifications en local
git checkout b && git pull
# Retourner sur la branche a
git checkout a
# Merge la branche b dans la branche a
git merge -m 'Merge branch b into a' b
```

Maintenant les dernières modifications de la branche b ont été fusionnées dans notre branche a. Allons modifier le fichier `index.js` pour prendre la variable globale du `fichierB.js`.

```javascript
import { showNstring } from "./src/fichierA.js";
import { variableImportante } from "./src/fichierB.js";

const result = showNstring(variableImportante);
console.log("Résultat : ", result);
```

Si nous relançons ce script avec la commande `node ./index.js` nous aurons le même résultat que précédemment mais avec les changements de notre collaborateur. Ajoutons notre modification dans un nouveau commit et pushons nos deux commit sur notre branche remote `git add . && git commit -m 'Push branch a v2' && git push`.

<div align="center">
<img align="center" alt="sshkey" src="https://utfs.io/f/8fca83dc-024d-49d5-9f04-6e2cb683f62c-f4jtt.png" />
<br>

_illustration branches git après le merge_

</div>

Et maintenant notre collaborateur veut publier sa branche sur la branche de production, et il le fait sans récupérer nos modifications avant et la!!!!!!! Hop hop hop, pas trop vite. C'est pour une [autre partie](./mergeconflicts.md) ça 😉 avant d'aborder les conflits, nous allons voir un outil qui permet de faciliter la gestion des branches en adoptant un workflow... le gitflow.

---

## In the next episode...

Dans la [partie suivante](./gitflow.md) nous allons approfondir le système de branches en utilisant les fonctionnalités gitflow qui permettent d'améliorer et de faciliter la gestion des branches et de suivre une façon plus idiomatique.

Si tu aimes ce contenu et que tu le trouves utile, n'hésite pas à le mettre dans tes favoris et à suivre [mon profil github](https://github.com/lbAntoine).

<div align="center">
<div>
<a href="./introduction.html">« Partie précédente</a>
•
<a href="/">Accueil</a>
•
<a href="./gitflow.html">Partie suivante »</a>
</div>
<img width="30%" src="https://utfs.io/f/35969b6d-f22c-4a41-9775-a54026f1ff73-mwy9q0.png" />
<div>
<a href="https://discordapp.com/users/328163554991669251" target="_blank">Discord</a>
•
<a href="https://linkedin.com/in/antoine-le-bras/" target="_blank">LinkedIn</a>
</div>
</div>
