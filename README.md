# Un TP en binôme

TP Binôme cpgeom

## Contexte du scénario

Vous êtes deux contributeurs sur un projet commun et vous embitionnez de créer un site de documentation.

Albert crée le projet et le publie sur GitHub.  
Bob le rejoint, apporte une modification, et Albert récupère le travail de Bob. À la fin, les deux voient le même historique sur GitHub.

> [!IMPORTANT]  On est ensemble, on apprend ensemble
> Albert et Bob vont s'assister mutuellement dans le cadre de cette mise en pratique

> Rôle A  - Albert  
Crée le projet, publie sur GitHub, récupère les contributions

> Rôle B  - Bob  
Clone le repo, ajoute un fiche de dco, pousse ses modifications

## 1 - Préparation — Compte & config Git

- Créer un compte GitHub (si pas encore fait)  
- Utiliser son compte GitHub -> Sign up  

Normalement déjà fait, sur son poste en local  
- Configurer son identité Git (1 seule fois par machine)  
```bash
git config --global user.name "Prénom Nom"  
git config --global user.email "email@exemple.com"  
git config --global user.name # vérifier 
```

## 2 - Albert — Créer et publier le projet

> Albert :  
Créer le repo local et un premier puis un deuxième fichier  

```bash
mkdir edc && cd edc
git init
echo "# La doc de l'edc" > README.md
echo "## La doc de l'utilisateur" > docutil.md
git add .
git commit -m "init : ajout README et doc utilisateur"
```

> Albert :  

Créer le repo distant sur GitHub

- Sur GitHub : + New repository -> nom "edc" -> Public -> sans cocher "Add README", ni .gitignore, ni LICENCE -> Create repository

> Albert :

> [!NOTE]
> Connecter le local au distant et pousser (cf page qui s'ouvre suite à la création de repo distant)
> ```bash
> git remote add origin https://github.com/Albert/edc.git
> git branch -M main
> git push -u origin main
> git remote -v # vérifier le remote
> ```

Aller sur GitHub et vérifier que les fichiers apparaissent dans le repo en ligne.

Donner l'URL du repo à Bob.  

## 3 - Bob — Cloner & contribuer

> Albert   
Ajouter Bob comme collaborateur

Settings -> Collaborators -> Add people -> entrer le username GitHub de Bob.  
Bob reçoit un email d'invitation à accepter.

> Bob   

Cloner le repo d'Albert en local

```bash
git clone https://github.com/Albert/edc.git
cd edc
ls # voir les fichiers récupérés
git log --oneline # voir les commits d'Albert
```

**Observer :**  
 git clone a automatiquement configuré origin -> vérifier avec git remote -v

> Bob 

Ajouter une nouvelle page de documentation sur les métadonnées par exmeple et pousser  

```bash
echo "## Métadonnées de la doc de l'EDC" > metadata.md
```

```bash
git status
git add metadata.md
git commit -m "Ajout des métadonnées"
git push origin main
```

## 4 - Albert récupère — exploration de l'historique
 
> Albert 

Récupérer le travail de Bob sans écraser ses modifications locales

```bash
git fetch origin # télécharger SANS fusionner
git log --oneline origin/main # voir les nouveaux commits
git pull origin main # fetch + merge
ls # metadata.md apparaît !
```

> [!IMPORTANT] Point clé
> faire fetch d'abord permet de voir ce qui arrive avant de l'intégrer. C'est une bonne pratique / habitude.


>Albert & Bob   

Explorer l'historique complet ensemble

```bash
git log --oneline --graph --all
git log --oneline --format="%h %an %s"
git show HEAD # détail du dernier commit
```

> [!CAUTION]  
Sur GitHub : aller dans Insights -> Network pour visualiser le graphe de commits. Les deux machines montrent-elles le même historique ?


> Albert & Bob 

Bonus — Albert modifie un fichier existant, Bob observe le diff

## Albert : édite docutil.md, commit et push

```bash
git diff 
HEAD~1 HEAD -- docutil.md # Bob après pull
```


## Erreurs fréquentes & solutions rapides  

``error: remote origin already exists``
-> git remote remove origin puis relancer add

``Authentication failed``  -> Utiliser un token HTTPS (Settings -> Developer tokens) ou configurer SSH

``rejected: non-fast-forward`` -> Faire git pull d'abord avant de push

``Please tell me who you are`` -> Configurer git config --global user.email

## Checklist de validation du TP

- [ ] Le repo d'Albert est visible sur github.com avec au moins 2 fichiers (Albert)
- [ ] Bob peut voir les commits d'Albert après git log (Bob)
- [ ] Le commit de Bob apparaît dans l'historique GitHub avec son username (Albert et Bob)
- [ ] Albert voit ratatouille.md après son git pull (Albert)
- [ ] git log --oneline --graph affiche 3 commits sur les deux machines (Albert et Bob)
- [ ] Le graphe GitHub Network montre les contributions des deux auteurs (Albert et Bob)

# Aller plus loin

Avec Github Pages et mkdocs ...