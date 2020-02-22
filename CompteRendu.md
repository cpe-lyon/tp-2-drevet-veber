# TP2 - Compte-Rendu

## Exercice 1 - Variables d’environnement

1. Dans la variable *PATH*
<br>\> `printenv PATH`
<br>`/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:[...]`

2. \>`printenv`
<br>`[...]`
<br>`HOME=/home/vebervi`
<br>`[...]`
<br>Il semble donc que ce soit la variable d'environnement *HOME* qui permette à la commande `cd` sans argument de nous ramener dans notre répertoire personnel.

3. - `LANG=fr_FR.UTF-8` - C'est la langue et l'encodage
	- `PWD=/home/` - C'est le dossier courant, elle change donc à chaque fois que l'on se déplace
	- `OLDPWD=/` - C'est donc le dossier où l'on était avant que l'on se trouve là où l'on est... C'est le dossier "précédent" de *PWD*. C'est cette variable qui est utilisée dans la commande `cd -` pour retourner au dossier où l'on était précedemment.
	- `SHELL=/bin/bash` - C'est là où se trouve l'interpréteur de commande, ici *bash*. Par ailleurs, on retrouve ce chemin au début des scripts bash.
	- `_=/usr/bin/printenv` - C'est le dernière paramètre de la dernière commande exécutée : 
	\> `touch 1.txt 2.txt 3.txt`
	\> `echo $_`
	`3.txt`

4. \> `MY_VAR="tp2ex1"`
<br>\> `echo $MY_VAR`
<br>`tp2ex1`

5. \> `bash`
<br>\> `echo $MY_VAR`
<br>*rien*
<br>`bash` ouvre un autre shell, d'où le fait que notre variable locale (donc uniquement connue du Shell précédent) n'existe plus.

6. \> `export MY_VAR="tp2ex1"`
<br>\> `echo $MY_VAR`
<br>`tp2ex1`
<br>\> `bash`
<br>\> `echo $MY_VAR`
<br>`tp2ex1`
<br>Notre variable existe encore car elle a bien été déclarée comme une variable d'environnement et donc accessible peu importe le shell dans lequel on se trouve.

7. \> `export NOMS="DREVET VEBER`
<br>\> `echo $NOMS`
<br>`DREVET VEBER`

8. \> `echo "Bonjour à vous deux, $NOMS !`
<br>`Bonjour à vous deux, DREVET VEBER !`

9. `unset` supprime la variable, elle n'existe plus. Affecter une chaîne vide à une variable crée quand même une variable, mais vide. Afficher une variable supprimée n'affichera rien tandis qu'afficher une variable vide donnera un retour à la ligne.

10. \> `echo '$HOME'" = $HOME"`
<br>`$HOME = /home/vebervi`

## Programmation Bash

Pour rajouter noter dossier *script* à la variable *PATH*, on va rajouter `PATH=$PATH:~/script` à la fin du fichier *.bashrc* puis on exécute la commande `source .bashrc` afin que la modification soit prise en compte.

*ne pas oublier le `chmod u+x script.sh` pour pouvoir exécuter le script*

### Exercice 2 - Contrôle de mot de passe

*testpwd.sh*
```
#!/bin/bash

PASSWORD=test
read -sp 'Entrez un mot de passe : ' usrPwd
if [ "$usrPwd" == "$PASSWORD" ]; then
	echo -e "\nC'est le bon mot de passe !"
else
	echo -e "\nCe n'est pas le bon mot de passe !"
fi
```

### Exercice 3 - Expressions rationnelles

*tp2ex3.sh*
```
#!/bin/bash

function is_number()
{
	re='^[+-]?[0-9]+([.][0-9]+)?$'
	if ! [[ $1 =~ $re ]] ; then
		echo 1
	else
		echo 0
	fi
}

if [[ $( is_number $1 ) -eq 0 ]]; then
	echo "C'est un nombre réel"
else
	echo "Ce n'est pas un nombre réel"
fi
```

### Exercice 4 - Contrôle d’utilisateur

*tp2ex4.sh*
```
#!/bin/bash

if [[ -z $1 ]]; then
	echo "Utilisation : $0 nom_utilisateur"
else
	if [[ -n $(cut -d : -f 1 /etc/passwd | grep ^$1$) ]]; then
		echo "Un utilisateur porte ce nom"
	else
		echo "Pas d'utilsateur trouvé portant ce nom"
	fi
fi
```

### Exercice 5 - Factorielle

*tp2ex5.sh*
```
#!/bin/bash

acc=1
for i in $(seq 1 $1)
do
	acc=$(( $i * $acc ))
done

echo "Factorielle $1 = $acc"
```

### Exercice 6 - Le juste prix

*tp2ex6.sh*
```
#!/bin/bash

price=$(( $RANDOM % 1000 ))
echo "Trouvez le juste prix entre 1 et 1000"

while [[ $price != $usrPrice ]]
do
	read -p "Votre prix : " usrPrice

	if [[ $price -gt $usrPrice ]]; then
			echo "C'est plus que $usrPrice"
	elif [[ $price -lt $usrPrice ]]; then
		echo "C'est moins que $usrPrice"
	fi
done

echo "C'est gagne ! Le juste prix etait $price"
```

### Exercice 7 - Statistiques

*tp2ex7.sh*
```
#!/bin/bash

function is_number()
{
	re='^[+-]?[0-9]+([.][0-9]+)?$'
	if ! [[ $1 =~ $re ]] ; then
		echo 1
	else
		echo 0
	fi
}

tab=()
while (("$#")) ;
do
	if [[ $( is_number $1 ) != 0 ]]; then
		echo "Un des nombres passés en paramètre nest pas un réel."
		exit
	fi

	tab+=($1)
	shift
done

acc=0 
min=${tab[0]}
max=${tab[0]}
for i in "${!tab[@]}"
do
	tmp=${tab[$i]}
	acc=$([ $tmp + $acc ))
	if [[ $tmp -lt $min ]]; then
		min=$tmp
	elif [[ $tmp -gt $max ]]; then
		max=$tmp
	fi
done

moy=$(( $acc / ${#tab[*]} ))

echo "Tableau : ${tab[*]}"
echo "Nombre de termes : ${#tab[*]}"
echo "Min : $min"
echo "Max : $max"
echo "Moyenne : $moy"
```

##### DREVET Yoann
##### VEBER Vincent
