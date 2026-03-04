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