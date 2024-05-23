## 1. Bases de git

### 1.2. .gitignore

Avant de nous attaquer aux branches et au travail en équipe, il est important de s'habituer aux bonnes pratiques. Le fichier .gitignore est une de ces bonnes pratiques qu'il faut absolument mettre en place et souvent même avant de faire notre premier push.

Il est bon de noter que beaucoup de frameworks ou CLI qui permettent de set-up des projets ajoutent souvent de base ce fichier à la racine du projet. Si il n'existe pas il faut penser à le créer. À l'intérieur on va renseigner les différents dossiers et fichiers qui sont intutiles de sauvegarder dans notre repo, ou alors que l'on ne veut pas mettre du tout (comme des mots de passes, des clés API etc...).

<div align="center">
<img align="center" alt="rootignore" src="./assets/treeignore.png"/>
<img align="center" alt="exignore" src="./assets/ignore.png"/>

_ci-dessus un exemple de .gitignore_

</div>

Au moment d'ajouter les fichiers pour notre commit git saura qu'il ne faut pas ajouter les fichiers et dossiers inscrits dans le _.gitignore_.

### 1.3. Les branches, travailler en équipe

Dans cette partie on va voir le concept des branches dans git. Mais avant on va revenir sur un concept de base qui a un rapport avec la première partie et qui a aussi sa place dans cette partie : `git clone` et `git pull`.

Il y a plus de chances, quand on est embauché dans une entreprise, qu'un projet soit déjà existant plutôt que l'on en soit à son origine. Ainsi un repository existe déjà. Pour pouvoir commencer à travailler sur ce projet il va falloir l'installer sur sa machine en local. C'est ce qui se passe quand on clone un projet (il existe autant de clone que de machine ayant le projet en local). Pour se faire, c'est comme quand on lie un projet local avec un repo. Tout d'abord on créé un dossier dans lequel on se rend avec le terminal, puis on copie le lien du projet en l'appliquant à cette commande :

```bash
~ git clone git@github.com:lbAntoine/git_memo.git

Cloning into 'git_memo'...
Enter passphrase for key '/c/Users/tomato/.ssh/id_ed25519':
remote: Enumerating objects: 7, done.
remote: Counting objects: 100% (7/7), done.
remote: Compressing objects: 100% (5/5), done.
remote: Total 7 (delta 0), reused 7 (delta 0), pack-reused 0
Receiving objects: 100% (7/7), done.
```

Et voilà qui est fait, on peut maintenant travailler sur le projet en local sur notre machine.

En plein milieu de notre point de dev, un de nos collègues envoie un message sur le canal d'équipe pour nous prévenir qu'il vient de mettre à jour le projet avec une fonction qui pourrait possiblement vous aider. Pour mettre à jour un projet, rien de plus simple. Dans le terminal (à la racine du projet comme toujours) :

```bash
~ git pull
Enter passphrase for key '/c/Users/tomato/.ssh/id_ed25519':
Already up to date.
```

Pour un peu plus de détails, `git pull` est une combinaison de `git fetch` (qui check si il y a des updates sur le remote) et `git merge` qui vient appliquer ces changements à votre local. Ce message juste au dessus, c'est si votre projet en local est déjà à jour, mais il peut y avoir d'autres messages. Attention !! Il est très mauvais de pull quand on a des changes sur notre local (arbre sale). Pour pouvoir mettre à jour notre local contenant des modifications sans tout casser il va falloir passer par un stash 😉 on ne prend pas peur, ça fait beaucoup de gros mots d'un coup mais c'est pas très compliqué. Tout d'abord le stash va nous permettre de mettre nos modifications de notre local dans un univers paralèlle le temps que nous fassions notre affaire, puis nous pourrons le réappliquer après. Pour se faire :

```bash
~ git stash
~ git pull
~ git stash pop
```

Avec l'enchainement des trois commandes ci-dessus on vient de nettoyer notre arbre, mettre à jour le projet puis réappliquer les changements que nous avions fait en local pour se remettre au travail (il faut veiller à exécuter chaque commande une par une).

Maintenant il est possible que vous soyez beaucoup à travailler sur le même projet (vraiment beaucoup), il est alors conseiller d'utiliser le système de branches de git. Ça peut faire assez peur parce que ça peut vite devenir le bazar, mais ça vaut vraiment le coup.

<img align="right" alt="gitbranches" width="40%" src="./assets/branches.png"/>
On peut retrouver la liste de nos branches directement sur notre repo github ou bien en ligne de commande si on tape :

```bash
~ git branch -a
* dev
  master
```

Cette commande peut être utile aussi pour savoir dans quelle branche nous sommes à ce moment (dans l'exemple nous sommes dans la branche dev). Pour changer de branche il faut utiliser `checkout`. Attention, quand on change de branche, c'est comme avec pull, il faut le faire avec un arbre propre. Il faut donc vérifier d'abord si nous n'avons pas de changements en local, et le cas échéant créer un `stash`, puis `checkout` et réappliquer le stash.

Pour créer une nouvelle branch, rien de plus simple :

```bash
~ git branch nom_nouvelle_branche
```

Et pour les plus avancé, les plus certains, les plus forts... on peut `checkout` directement dans une nouvelle branche avec le flag `-b`.

```bash
~ git checkout -b nom_nouvelle_branche
Switched to a new branch 'nom_nouvelle_branche'
```

Ensuite avec tout ce concept on pourrait dire "Ouais mais heu à quoi ça sert de séparer le projet autant, comment mon site va fonctionner si il est autant éparpillé ? Comment je fais ? Chouine chouine chouine...", laisse moi terminer 😊 maintenant on prend nos lunettes, on met des gants, on s'attaque au `merge`.

On a enfin terminé notre fonction, on l'a bien testée, on est enfin prêt à la déployer sur notre application ! On veille bien d'abord à `commit` nos derniers changements et on se tient prêt. On va d'abord se rendre sur notre branche cible.

```bash
~ git checkout master
```

Puis on va quand même vérifier que cette branche principale est à jour sur notre local.

```bash
~ git pull
```

Maintenant on va merge notre branche sur laquelle nous avons développé notre fonction sur la branche principale pour intégrer cette fonction en production.

```bash
~ git merge nom_de_la_branche
```

Étant donné que nous sommes déjà sur la branche cible il n'y rien à préciser de plus, git se charge du reste. Maintenant si besoin est nous pouvons supprimer la branche de développement de notre fonction si elle n'est plus utile :

```bash
~ git -d nom_de_la_branche
```

Et puis nous pouvons `push` tout ça et laisser profiter nos collègues de notre super boulot 😊 bien joué !

Pour des problèmes ou d'autres stratégie de merge, je laisse la parole à la documentation officielle, bien que si ce mémo est bien appliqué, il ne devrait pas y avoir d'erreurs (sauf si une autre stratégie de merge doit être utilisée plutôt que le _fast-forward_ utilisé ici).

## 2. Gitflow

_En cours d'écriture_

## 3. Merge et conflits

_En cours d'écriture_

## 4. Level up ton terminal

_En cours d'écriture_

## 5. GH CLI (spécificités github)

_En cours d'écriture_