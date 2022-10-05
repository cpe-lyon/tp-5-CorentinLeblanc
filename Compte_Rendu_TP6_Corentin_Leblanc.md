**Compte rendu TP 5 --Gestion des disques**

1 -- Je me suis rendu dans l'interface de configuration des machines virtuelles et j'ai ajouté un nouveau disque dur de 5Go en allocation dynamique. L'allocation dynamique est importante car elle permet d'utiliser uniquement l'espace nécessaire.(Exemple -> Si je créer une VM de 20 Go dynamiquement alloué alors le système prendra uniquement l'espace dont il a besoin et agrandira son espace au fur et a mesure. A contrario, une machine non dynamiquement alloué reservera directement 20Go qui deviendront inutilisables par la suite.)

![image](https://user-images.githubusercontent.com/104362418/194068251-11370bbb-2b32-4a5d-875e-9700e13820d4.png)

2 -- Grâce à la commande "lsblk" j'ai pu vérifié que mon nouveau disque dur à bien été ajoutée.

![image](https://user-images.githubusercontent.com/104362418/194069211-d2e95218-c09c-49cb-a041-796fe4c61717.png)

On vois sur cette capture le premier disque dur avec ses différentes partitions(40 Go), puis en dessous on retrouve le deuxième disque dur(5 Go).

3 -- Voici la création des partitions demandées

![image](https://user-images.githubusercontent.com/104362418/194076349-551e8f95-68f2-442c-bffc-b1e00091af33.png)

Ci-dessus la création de la première partition de 2 Go de type 'Linux', j'ai pu créer cette partition dans l'interface "sudo fdisk /dev/sdb".

![image](https://user-images.githubusercontent.com/104362418/194077747-5215bfca-750e-4caf-808c-14b3a6b2caa5.png)

Voici la seconde partition créer au format NTFS(extended sous Linux).
Si je listes toutes les partitions du disques, je constate qu'il y a bien les deux partitions que je viens de créer.

![image](https://user-images.githubusercontent.com/104362418/194078017-9e830773-f1ad-4aaa-aca8-98138d44ea60.png)

4 -- 

