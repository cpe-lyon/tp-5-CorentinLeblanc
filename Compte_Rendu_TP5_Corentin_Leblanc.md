**Compte rendu TP 5 --Gestion des disques**

**<ins>Exercice 1. Disques et partitions</ins>**

1 -- Je me suis rendu dans l'interface de configuration des machines virtuelles et j'ai ajouté un nouveau disque dur de 5Go en allocation dynamique. L'allocation dynamique est importante car elle permet d'utiliser uniquement l'espace nécessaire.(Exemple -> Si je créer une VM de 20 Go dynamiquement alloué alors le système prendra uniquement l'espace dont il a besoin et agrandira son espace au fur et a mesure. A contrario, une machine non dynamiquement alloué reservera directement 20Go qui deviendront inutilisables par la suite.)

![image](https://user-images.githubusercontent.com/104362418/194068251-11370bbb-2b32-4a5d-875e-9700e13820d4.png)

2 -- Grâce à la commande **"lsblk"** j'ai pu vérifié que mon nouveau disque dur à bien été ajoutée.

![image](https://user-images.githubusercontent.com/104362418/194069211-d2e95218-c09c-49cb-a041-796fe4c61717.png)

On vois sur cette capture le premier disque dur avec ses différentes partitions(40 Go), puis en dessous on retrouve le deuxième disque dur(5 Go).
J'aurais également pu utilisé "**fdisk -l**".

3 -- Voici la création des partitions demandées

![image](https://user-images.githubusercontent.com/104362418/194076349-551e8f95-68f2-442c-bffc-b1e00091af33.png)

Ci-dessus la création de la première partition de 2 Go de type 'Linux', j'ai pu créer cette partition dans l'interface **"sudo fdisk /dev/sdb"**.(On peut mettre +2G pour définir une partition de 2 Giga et se simplifier la tâche)

![image](https://user-images.githubusercontent.com/104362418/194077747-5215bfca-750e-4caf-808c-14b3a6b2caa5.png)

Voici la seconde partition créer au format NTFS(extended sous Linux).
Si je listes toutes les partitions du disques, je constate qu'il y a bien les deux partitions que je viens de créer.

![image](https://user-images.githubusercontent.com/104362418/194078017-9e830773-f1ad-4aaa-aca8-98138d44ea60.png)

4 -- J'ai formaté mes deux partitions au format ntfs grâce à la commande **"mkfs"**.

![image](https://user-images.githubusercontent.com/104362418/194229864-592e40e7-b495-4453-a96b-6a55ec666fb4.png)

Les commandes à utilisé sont "**mkfs.ext4 /dev/sdb1**" pour la première et "**mkfs.ntfs /dev/sdb2**" pour la seconde partition.

5 -- La commande **"df -T"** ne fonctionne pas sur notre disque car il n'a pas encore été monté.

![image](https://user-images.githubusercontent.com/104362418/194227998-696ca1b5-db27-41d0-81e8-9bdb0c08241f.png)

6 -- J'ai ajouté ces deux lignes dans le fichier **"/etc/fstab"**, il contient le nécessaire pour automatiser le montage des partitions.

![image](https://user-images.githubusercontent.com/104362418/194230647-29d71004-275c-46e5-82ba-e13448f9939b.png)

Puis pour valider j'ai fais "**mount -a**" puis "**mount**".

7 -- J'ai utilisé la commande ci-dessous pour monter les partitions puis j'ai reboot ma machine.

![image](https://user-images.githubusercontent.com/104362418/194231865-58d07eb9-59bc-461c-ae86-ff638354375b.png)

On retrouve l'option **"-a"** qui indique l'on souhaite monter tout les systèmes de fichier mentionnés dans le fichier **"fstab"**,
il y a ensuite le numéro de la partition puis sur le point de montage.

8 -- Pour monter une clé USB dans ma VM, il faut passer par la Virtual remote console.(Nous n'y avons pas accès, il faut un compte VMWare).

9 -- Pour créer un dossier partagé entre ma VM et mon système hôte, j'ai trouvé ce lien qui peut etre intéressant:
https://openclassrooms.com/fr/courses/2356316-montez-un-serveur-de-fichiers-sous-linux/5173631-partagez-vos-fichiers-sur-un-reseau-linux-avec-nfs

<ins>**Exercice 2. Partitionnement LVM**</ins>

1 - Voici comment j'ai démonté les systèmes de fichiers qui étaient montés dans /data et /win, /data n'était plus monté.

![image](https://user-images.githubusercontent.com/104362418/194235487-ff5cbf01-e6d4-458b-8ac1-d1ac9831c8a8.png)

Puis je suis allé supprimer les lignes correspondantes dans le fichier **"/etc/fstab"**.

2 -- La suppression des partitions à été réalisée avec la commande **"sudo fdisk /dev/sdb"** puis l'option "d" ainsi que le numéro de la partition que je souhaite supprimer.

![image](https://user-images.githubusercontent.com/104362418/194236716-00a7c69a-c89f-497c-be7e-eb8632b9f55a.png)

Si je regarde les partitions qui se trouvent dans mon disque je constate qu'il n'y en a aucune.

![image](https://user-images.githubusercontent.com/104362418/194236841-0e36aa8a-e59e-4726-b81e-888d2a0f5b95.png)

J'ai ensuite créé une partition unique.

![image](https://user-images.githubusercontent.com/104362418/194238424-2eb357ce-9a85-431c-844c-d4e1a49006e1.png)

3 -- La commande **"pvcreate"** m'a bien permis de créer un volume physique LVM. La commande **"pvdisplay"** m'indique qu'il a bien été créé.

![image](https://user-images.githubusercontent.com/104362418/194242133-fb5e2f69-8ee1-4cd1-90a2-511b5f1c0e8d.png)

4 -- Même chose que la question précédente mais avec la commande **"vgcreate"** et **"vgdisplay"**

![image](https://user-images.githubusercontent.com/104362418/194242742-8d119653-b232-41ba-9444-8c58386cbc8e.png)

5 -- Je me suis servis de l'option **"-l 100%FREE"** pour utiliser la totalité de l'espace libre puis **"-n lvData"** pour lui donner le nom demandé.

![image](https://user-images.githubusercontent.com/104362418/194243387-c024ef61-add0-4151-96ad-2ec01a778fdf.png)

6 -- Voici la création de la partition qui à été formaté en ext4.

![image](https://user-images.githubusercontent.com/104362418/194244917-89dde743-ee69-488f-9191-40ebe27be3e5.png)

J'ai ensuite fait en sorte qu'il soit monté de manière automatique au démarrage de la machine dans **"/data"*.
Pour ceci, je me suis servis des réponses que j'avais données aux questions 6 et 7.

On se sert également des commandes "**fdisk /dev/mapper/vg00-lvData**" et "**mkfs.ext4 /dev/mapper/vg00-lvData**".

7 -- Pour cette question, je me suis servis de la question 1 de l'exercice 1 ainsi que des questions 2 et 3 de l'exercice 2.

![image](https://user-images.githubusercontent.com/104362418/194246638-107cda41-8a92-41eb-bbae-abefff95283d.png)

Le disque est bien présent.

![image](https://user-images.githubusercontent.com/104362418/194247264-ce9c7d96-c095-42b0-a118-efa3b2b94bf2.png)

Après avoir créer une partition unique, j'ai créé un volume physique, je constate qu'il est bien présent avec la commande "pvdisplay".

![image](https://user-images.githubusercontent.com/104362418/194247506-7acac2d1-0395-4957-8eab-735424c7216a.png)

On peut aussi se servir de "**fdisk /dev/sdc**" et de "**pvcreate /dev/sdc1**".

8 -- Voici l'ajout du nouveau disque au groupe de volume

![image](https://user-images.githubusercontent.com/104362418/194248741-79df5617-ba1f-47c5-8506-8143de578786.png)

9 -- Voici les commandes pour agrandir le volume logique:

sudo lvresize -l 2046 /dev/mapper/vg00-lvData (2046 correspond à 8Gio)

sudo e2fsck -f /dev/mapper/vg00-lvData

sudo resize2fs /dev/mapper/vg00-lvData




