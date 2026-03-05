vcs for tracking changes in computer files

No specific language or framework for git

distibuted / decentralized version control : developers don't need to be on the same network to use

coordinates between developers and track versions so that we can return to any specific version as long as it is committed to the repo

devs make changes to their local repo and push it the the remot repo ( ex : github , bitbucket , )

no internet needed to make changes to local repo unless you want to push to the remote repo



git keeps track of code history with snapshots of files by making commit 

dev can see previous snapshots and revert back

==> safe code

code can be in a staging area before doing commit to a snapshot ( git add command )

once a dev pushes code to a remote repo , other devs can pull into their machines ( hence the concept of branches)



git basic commandes :

** git init : initialize a git repo in current folder + create .git folder ( hidden by default and never touched for most of the time by devs ), once we do this command , we can do other commands

** git add <file> : add file to the staging area to be ready for commit ( dev axecute this command as much as they can , no prob)

** git status : check files in the staging area ans display difference between the working tree and the staging area 

** git commit : take everything in the index / staging area to the local repo

The next commands have to do with local repo :

** git push : push local repo to remote repo ( add also credentials , ssh keys to not add password )

** git pull : pull changes from remote repo to local repo

** git clone : download a project on remote repo to a dev's machine



git installation : straightforward with linux ( debian / fedora ) or mac or windows ( there is git bash for a linux-likely environment in windows , and gui tools for git are not recommended , it is better to always use commands)



git --version

cd <folder>

git init

git config --global user.name ''

git config --global user.email ''

git add <file>

git status

git rm --cached <file>

git add .

git commit

git commit -m "message"

touch .gitignore ( for files and folders that dev don't want them to be included in the staging area)




concept of branches : to avoid pushing to the main code without finishing the functionality, so dev create branch

git branch <branch2-name>

git checkout <branch2-name>

git merge <branch2> : to merge second branch to current branch




git commands for remote repo : 

git remote add origin <url>

git remote

git push -u origin <main-branch>

git push

git clone <url-git>

git pull





git gère les tags ( pour version / attaché à un commit )

** git tag

git gère conflits et merges



git help <commande>

repo c le dossier .git

working copy / working tree

git checkout -- <file > : annule les modifs d un fichier

git commit -a -m "message"

git commit --amend

git log  -n -p --oneline

git show --stat

git diff [id_commit]

id_commit^
id_commit^^
id_commit~n
id_commit^2
id_commit1..id_commit2

feature branch

git branch

git checkout -b <nom-branche> : création + se positionner dessus

git branch -d mabranche ( supprimer branche si mergé )

git branch -D mabranche ( forcer suppression / recuperation via reflog )

branche^^
tag^^

git checkout <ref> : branche , tag , commit ...

git checkout ne fontionne pas si des fichiers non commités modifiés

git reset ( reset index et working copy )
git reset [id_commit ] : le head se positionne sur id_commit 
--mixed ( par défaut ) : reset index
--soft : non
--hard : reset index + working copy


git tags ( liste des tags )

git tag -a nom_du_tag -m "message" ( créer un tag )

checkout possible avec tag ( car c une référence vers un commit / aussi les notations tag^^ tag~)



reflog :
enregistre tous les mouvements de head ( recuperation max 30 jours )

git reflog ( affiche l historique des positions de head)

git reset --hard HEAD@{n} ( pour revenir par exemple vers un commit supprimé)



merge 

fast forward ( A - B / A - B - C -D  ==> A - B - C - D)
non fast forward ( A - B - C / A - B - E - D ==> A - B - M commit de merge )

conflit ( exemple modification de la même ligne) :
modifiy file -> git add file -> git commit
git mergetool ( outil graphique )
git merge --abort ( annule merge)
git reset --hard HEAD^ (revient a l etat avant merge)

git merge feature --no-ff (git force la création d'un commit de merge)
A - B / A - B - C - D ==> A - B - M



rebase ( prendre les commits d'une branche et les rejouer après une autre branche )
git checkout branch1 ( A - B - C - D )
git rebase master ( A - B - E - F)
==> prendre les commits de branch1 et les mettre au dessus de master ( A - B - E - F - C' - D') 


difference entre merge et rebase :
git rebase master ( mettre à jour une branche )
git merge feature ( fusionner une feature )


fork ( copie du repo read only dans le compte du dev)
heroku ( exemple pour déployer un code )



bare repo ( repo sans fichiers de projet working copy : .git contents only )
git init --bare ( crée project.git sans working directory)
git clone --bare <url> ( clone un repo sans fichiers du projet)


remote branch
git branch -a ( liste les branches locales et remotes)

git fetch <remote> ( télécharge les nouvelles infos du repo distant : commits , branches , mise a jour des branches mais ne modifie pas code local)
pull = fetch + merge


fetch + rebase === git pull --release

git push
git status

git push nom_remote id_commit: nom_branche_remote ( par défaut git push publie jusqu au dernier commit + ici on publie jusqu a un commit)

git push -f 
explication : avec git push
repo distant : A - B - C 
repo local : A - B - D
==> git refuse car c diff de d
avec -f : repo distant A - B - D
alternative : git revert ( annule commit sans modifier historique )


comment créer branche remote ?
git checkout -b mabranche
git push -u origin maBranche ( -u == --set-upstream ca crée un lien entre la branche locale ela branche distante )

git checkout --track origin/nombranche ( cree nombranche comme branche locale et liée à origin/payment )

git push nomRemote :nomBranche ( supprimer branche distante )

git push nomremote nomtag( publier tag sur serveur)

git revert id_commit ( annule modif du commit)

git blame file ( indiquer auteur de chaque ligne de file )

git stash ( git cache les modifs et remets dans l etat du dernier commit , ex: changement de branche )

git stash list ( affiche la liste des stash )

git stash pop stash@{1} ( supprime la stash )

bisect ( si dev ne sait pas quelle version precise a commit le bug mais on sait 2 good and bad)
git bisect start ( démarre)
git bisect good <ref>
git bisect bad <ref>
git bisect skip <ref>
git bisect visualize ( affiohe les suspects restants )

git grep <texte> <ref>

git add -p ( plsrs modifs dans un modif que dev pour une partie pour un commit et une atre pour un autre commit )
git gui ( interface pour selectionner des hunks)

git cheery-pick <commit> ( pour recuperer un commit precis contrairement à merge ou rebase )

git format-patch -<nombre_commits> (creer patch : fichier qui contient les différenes entre 2 versions , si avec nombre_commit ca prepare nombre_commit patches )
git apply patchfile.patch ( recuperer les modifs )


