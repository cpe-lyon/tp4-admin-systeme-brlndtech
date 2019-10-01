# Compte rendu TP4  <img src="https://image.flaticon.com/icons/svg/518/518713.svg" height="50" alt="Zozor" /> | Geoffrey Sauvageot-Berland 

## Exercice 1. Gestion des utilisateurs et des groupes

### 1. Commencez par créer deux groupes groupe1 et groupe2

<code> addgroup groupe1 </code>
<code> addgroup groupe2 </code>

### 2. Créez ensuite 4 utilisateurs u1, u2, u3, u4 avec la commande useradd, en demandant la création de leur dossier personnel et avec bash pour shell


```
useradd -m u1
useradd -m u2
useradd -m u3
useradd -m u4
```
### 3. Placez les utilisateurs dans les groupes :

usermod -a -G groupe1 u1
usermod -a -G groupe1 u2
usermod -a -G groupe1 u4

<br>

usermod -a -G groupe2 u2
usermod -a -G groupe2 u3
usermod -a -G groupe2 u4

### 5. Donnez deux moyens d’afficher les membres de groupe2 
```
cat /etc/group |grep groupe2
groupe2:x:1002:u2,u3,u4

grep groupe2 /etc/group
groupe2:x:1002:u2,u3,u4
```

### 6. Faites de groupe1 le groupe propriétaire de /home/u1 et /home/u2 et de groupe2 le groupe propriétaire de /home/u3 et /home/u4
```
usermod -g groupe1 u1 
usermod -g groupe1 u2
usermod -g groupe2 u3
usermod -g groupe2 u4
```
### 7. Créez deux répertoires/home/groupe1et/home/groupe2pour le contenu commun aux groupes, etmettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossierassocié

mkdir /home/groupe1
mkdir /home/groupe2

chown u1:groupe1 groupe1
chown u2:groupe2 groupe2

chmod 720 groupe1 groupe2

### 8. Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer   ou supprimer ce fichier ?

Il faut donner des droits d'écriture (w) au propriétaire. 
Je propose un un chmod 755

### Pouvez-vous vous connecter en tant que u1 ? Pourquoi ?

Le compte est désactivé, car nous n'avons pas paramétrer son mdp en amont,lors de la création du compte. 

### Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son compte
```
passwd u1
su u1
```

### Quels sont l’uid et le gid de u1 ?
```

première option : 

id u1
uid=1001(u1) gid=1001(groupe1) groups=1001(groupe1)

deuxième option : 

cat /etc/passwd (regarder vers le compte souhaité u1)
```

### Quel utilisateur a pour uid 1003 ?

```
première option : 

id u3
uid=1003(u3) gid=1002(groupe2) groups=1002(groupe2)

deuxième option : 

cat /etc/passwd (regarder vers le compte souhaité u3)

```

### Quel est l’id du groupe groupe1 ?

```
cat /etc/group | grep "groupe1"
groupe1:x:1001:u1,u2,u4
```

### Quel groupe a pour gid 1002 ? ?

```
cat /etc/group | grep "groupe2"
groupe1:x:1002:u2,u3,u4
```

### Retirez l’utilisateur u3 du groupe groupe2. Que se passe-t-il ? Expliquez 

```
nano /etc/group
groupe2:x:1002:u2,u4 (j'ai enlevé manuellement u3)

u3 n'est plus en mésure de modifier le contenu du dossier /home/groupe2 et d'y accéder. 

```

### Modifiez le compte de u4 de sorte que :
— il expire au 1er juin 2019
— il faut changer de mot de passe avant 90 jours
— il faut attendre 5 jours pour modifier un mot de passe
— l’utilisateur est averti 14 jours avant l’expiration de son mot de passe
— le compte sera bloqué 30 jours après expiration du mot de passe

- sudo chage -E 18048 u4
- sudo chage -M 90 u4
- sudo chage -m 5 u4
- sudo chage -W 14 u4
- sudo chage -I 30 u4


### Quel est l’interpréteur de commandes (Shell) de l’utilisateur root ?

```
(en mode root)
echo $SHELL 
/bin/bash (sh plus récent = bourne against shell)

(en mode utilisateur sans droits)
echo $SHELL 
/bin/sh (ancien shell)

On peut aussi visualiser le type de shell d'une personnne via 

cat /etc/passwd 
<regarder la ligne pour l'utilisateur souhaité>

exemple pour root : root:x:0:0:root:/root: **/bin/sh**
```

### à quoi correspond l’utilisateur nobody ?

En linux : nobody  est le nom conventionnel d'un compte d'utilisateur à qui aucun fichier n'appartient, qui n'est dans aucun groupe qui a des privilèges et dont les seules possibilités sont celles que tous les "autres utilisateurs" ont.

### Par défaut, combien de temps la commandesudoconserve-t-elle votre mot de passe en mémoire?Quelle commande permet de forcersudoà oublier votre mot de passe?

Je ne le savais pas de base, alors j'ai consulté le manuel pour la commande sudo. 

man sudo 
/minutes

-> j'ai trouvé 15 minutes pour la validité du mdp.
Le mot de passe est donc valide pendant 15 minutes, pour un utilisateur "Sudoer"











