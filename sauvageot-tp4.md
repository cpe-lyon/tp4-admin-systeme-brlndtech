# Compte rendu TP4  <img src="https://image.flaticon.com/icons/svg/518/518713.svg" height="50" alt="Zozor" /> | Geoffrey Sauvageot-Berland 

## Exercice 1. Gestion des utilisateurs et des groupes

### Commencez par créer deux groupes groupe1 et groupe2

<code> addgroup groupe1 </code>
<code> addgroup groupe2 </code>

### Créez ensuite 4 utilisateurs u1, u2, u3, u4 avec la commande useradd, en demandant la création de leur dossier personnel et avec bash pour shell


```
useradd -m u1
useradd -m u2
useradd -m u3
useradd -m u4
```
### Placez les utilisateurs dans les groupes :

usermod -a -G groupe1 u1
usermod -a -G groupe1 u2
usermod -a -G groupe1 u4

<br>

usermod -a -G groupe2 u2
usermod -a -G groupe2 u3
usermod -a -G groupe2 u4

### Donnez deux moyens d’afficher les membres de groupe2 
```
cat /etc/group |grep groupe2
groupe2:x:1002:u2,u3,u4

grep groupe2 /etc/group
groupe2:x:1002:u2,u3,u4
```

### Faites de groupe1 le groupe propriétaire de /home/u1 et /home/u2 et de groupe2 le groupe propriétaire de /home/u3 et /home/u4
```
usermod -g groupe1 u1 
usermod -g groupe1 u2
usermod -g groupe2 u3
usermod -g groupe2 u4
```

### Faites de groupe1 le groupe propriétaire de /home/u1 et /home/u2 et de groupe2 le groupe propriétaire de /home/u3 et /home/u4

mkdir /home/groupe1
mkdir /home/groupe2

chown u1:groupe1 groupe1
chown u2:groupe2 groupe2

chmod 720 groupe1 groupe2

### Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer   ou supprimer ce fichier ?

### Pouvez-vous vous connecter en tant que u1 ? Pourquoi ?

```
su u1
``` 
### Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son compte
```
passwd u1
```

### Quels sont l’uid et le gid de u1 ?
```
id u1
uid=1001(u1) gid=1001(groupe1) groups=1001(groupe1)
```

### Quel utilisateur a pour uid 1003 ?

```
u3
uid=1003(u3) gid=1002(groupe2) groups=1002(groupe2)

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













