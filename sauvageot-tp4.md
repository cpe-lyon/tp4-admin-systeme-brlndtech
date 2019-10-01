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

ou getent group groupe1

```

### Quel groupe a pour gid 1002 ? ?

```
cat /etc/group | grep "groupe2"
groupe1:x:1002:u2,u3,u4

ou getent group 1002
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

## Exercice 2 

### 1. Dans votre $HOME, créez un dossier test, et dans ce dossier un fichier fichier contenant quelques lignes de texte. Quels sont les droits sur test et fichier ?

```
cd ~
mkdir test 
cd test 
nano fichier 

Dossier test : 755 (droits de base pour un dossier sous linux)
fichier fichier : 644 (droits de base pour un fichier sous linux)

```

### 2. Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’aﬀicher entant queroot. Conclusion?
```
chmod 000 fichier 
```

Nous pouvons toujours accéder au fichier en root, et le modifier. Normal nous sommes le superuser.

### 3. Redonnez vous les droits en écriture et exécution surfichierpuis exécutez la commandeecho "echoHello" > fichier. On a vu lors des TP précédents que cette commande remplace le contenu d’unfichier s’il existe déjà. Que peut-on dire au sujet des droits
```
chmod 300 fichier 
echo "echo hello world" > fichier
```
Il nous est possible d'écrire dans ce fichier fichier. Le contenu précédent à été remplacé par : cho hello world

### 4. Essayez d’exécuter le fichier. Est-ce que cela fonctionne ? Et avec sudo ? Expliquez.

Cela ne va pas fonctionner car nous n'avons pas les droits d'éxécution de celui ci. Sauf si nous sommes en root. 

```
bash: ./fichier: Permission denied
sudo ./fichier
``` 
### 5. Placez-vous dans le répertoiretest, et retirez-vous le droit en lecture pour ce répertoire. Listez lecontenu du répertoire, puis exécutez ou aﬀichez le contenu du fichierfichier. Qu’en déduisez-vous?Rétablissez le droit en lecture surtest


```
sudo chmod u-r ../test
ls 
cannot open directory '.': Permission denied
./fichier 
bash: ./fichier: Permission denied
```

Impossible d'afficher le contenu du dossier ni d'éxécuter le fichier. Normal nous n'avons plus les droits.

### 6. Créez dans test un fichiernouveauainsi qu’un répertoiresstest. Retirez au fichiernouveauet aurépertoiretestle droit en écriture. Tentez de modifier le fichiernouveau. Rétablissez ensuite le droiten écriture au répertoiretest. Tentez de modifier le fichiernouveau, puis de le supprimer. Que pouvez-vous déduire de toutes ces manipulations?
```
nano nouveau
mkdir sstest
chmod u-w nouveau
chmod u-w ../test
nano nouveau
[ Error writing nouveau: Permission denied ]
chmod u+w ../test
nano nouveau
[ Error writing nouveau: Permission denied ] 
rm nouveau
rm: remove write-protected regular file 'nouveau'? yes
ls fichier  sstest
```
Nous pouvons constater que les fichiers n'hérite pas des droits d'un dossier (si on ne le param pas) Ils ont des droits spécifique.

### 7. Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoire test. Tentez de créer, supprimer, ou modifier un fichier dans le répertoire test, de vous y déplacer, d’en lister le contenu, etc…Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires ?
```
 chmod u-x test
 nano test/nouveau
[ Path 'test' is not accessible ]
 cd test/
-bash: cd: test/: Permission denied
ls test
ls: cannot access 'test/sstest': Permission denied
ls: cannot access 'test/fichier': Permission denied
fichier  sstest
```

Nous ne pouvons éffectuer aucune de ces actions, car nous sommes bloquer par les droits d'éxécution qui ont été retiré.

### 8. Rétablissez le droit en exécution du répertoire test. Attribuez au fichier fichier les droits suffisants pour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.

```
chmod 640 test

```

### 9. Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture, ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.

```
umask 022 test 

```

### 10. Définissez un umask équilibré qui vous autorise un accès complet et autorise un accès en lecture aux membres de votre groupe. Testez sur un nouveau fichier et un nouveau répertoire.

```

umask 037 test 

``` 

### 11. Transcrivez les commandes suivantes de la notation classique à la notation octale ou vice-versa (vouspourrez vous aider de la commandestatpour valider vos réponses) :

```
chmod u=rx,g=wx,o=r fic = chmod 534 -r-x--wx-r--
chmod uo+w,g-rx fic = chmod 706 -rwx-x-rw-

``` 
### Affichez les droits sur le programme passwd. Que remarquez-vous ? En affichant les droits du fichier /etc/passwd, pouvez-vous justifier les permissions sur le programme passwd ?

```
cd /etc 
ls -l passwd    
-rw-r--r-- 1 root root 1801 sept. 30 21:15 /etc/passwd

```

Le fichier est accessible par le prop en lecture / ecriture, et est juste lisible pour les membres du groupe de l'owner et les autres. 

Il est tout a fait normal que les autres et le groupe ne puisse pas modifier ce fichiers, qui contient un ensemble de données critique concernant l'architecture d'un réseau UNIX.