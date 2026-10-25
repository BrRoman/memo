# Mini scripts shell

##### Ces scripts ont pour but de montrer la syntaxe des scripts shell

et la manière d'y utiliser des boucles, conditions, redirections, paramètres et enchaînements de commandes.

#### SUBSTRINGS

**Substring removal:**

**${string#substring}**: Deletes shortest match of `$substring` from front of `$string`.

**${string##substring}**: Deletes longest match of `$substring` from front of `$string`.

**${string%substring}**: Deletes shortest match of `$substring` from back of `$string`.

**${string%%substring}**: Deletes longest match of `$substring` from back of `$string`.

**Examples:**

```bash
url="http://clairval.com"
echo ${url#*/}
>>> /clairval.com
echo ${url##*/}
>>> clairval.com
echo ${url%/*}
>>> http:/
echo ${url%%/*}
>>> http:
url="http://clairval.com/"
echo ${url#*/}
>>> /clairval.com/
echo ${url##*/}
>>>
echo ${url%/*}
>>> http://clairval.com
echo ${url%%/*}
>>> http:
```

Avec `cut`:

```bash
$ s='one_two_three_four_five'
$ A="$(cut -d'_' -f2 <<<"$s")"
$ echo "$A"
two
$ B="$(cut -d'_' -f4 <<<"$s")"
$ echo "$B"
four
```

Avec `awk`:

```bash
A=$(awk -F_ '{print $2}' <<< 'one_two_three_four_five')
B=$(awk -F_ '{print $4}' <<< 'one_two_three_four_five')
```

Remove `n` characters from a string:

```bash
a=123
echo "${a::-1}"
=> 12
```

#### RENOMMER DES FICHIERS

(Voir aussi la commande "find" dans la page "Commandes".)

Le script suivant change tous les fichiers php en fichiers html dans le dossier mon_dossier :

```bash
#!/bin/bash
cd '/chemin/vers/mon_dossier'
find . -type f -name '*.php' | sort | while read
do
f=`echo "$REPLY"`
fn=`echo "$REPLY" | sed 's/php/html/g'`
#echo "$fn"
mv "$f" "$fn"
done
```

Plus simple :

```bash
# Rename all *.txt to *.text
for file in *.txt; do
mv -- "$file" "${file%.txt}.text"
done
```

Ou encore :

```bash
for file in *.txt; do
mv -- "$file" "$(basename -- "$file" .txt).text"
done
```

Voir aussi l'utilitaire rename:

```bash
Given the files foo1, ..., foo9, foo10, ..., foo278, the commands
rename foo foo00 foo?
rename foo foo0 foo??
will turn them into foo001, ..., foo009, foo010, ..., foo278.

And
rename .htm .html *.htm
will fix the extension of your html files.

Provide an empty string for shortening:
rename '_with_long_name' '' file_with_long_name.*
will remove the substring in the filenames.
```

#### TEST SUR UNE LISTE

Le script suivant cherche si le paramètre $1 est dans la liste [cat, dog, mouse].

```bash
case "$1" in "cat"|"dog"|"mouse")
echo "dollar 1 is either a cat or a dog or a mouse" ;;
*) echo "none of the above" ;;
esac
```

#### TESTS EN CASCADE

```bash
#!/bin/bash
NOW=$(date -d "$1" +"%a")
case $NOW in
Mon)
echo "Mon";;
Tue|Wed|Thu|Fri)
echo "Tue|Wed|Thu|Fri";;&
Fri|Sat|Sun)
echo "Fri|Sat|Sun";;
*) ;;
esac
```

Commentaire :
Le & fait tester le statement suivant, même si le précédent matche.

```bash
$ ./myscript thursday
Tue|Wed|Thu|Fri
```

```bash
$ ./myscript friday
Tue|Wed|Thu|Fri
Fri|Sat|Sun
```

```bash
$ ./myscript saturday
Fri|Sat|Sun
```

#### EXPANSION DE VARIABLES

```bash
var=test
test="my string"
echo "$var"
=> test
echo "${!var}"
=> my string
```

#### REMPLACEMENTS MULTIPLES SUR UNE SÉRIE DE FICHIERS

Le script suivant effectue un rechercher/remplacer sur tous les fichiers de mon_dossier dont l'extension est '.txt' ou '.tex' ou '.gabc'.

```bash
#!/bin/bash
cd /chemin/vers/mon_dossier
find . \( -name '*.txt' -o -name '*.tex' -o -name '*.gabc' \) | while read
do
sed -i "s/√/↓/g" "$REPLY"
sed -i "s/®/¶/g" "$REPLY"
sed -i "s/†/ŧ/g" "$REPLY"
sed -i "s/ã/å/g" "$REPLY"
sed -i "s/õ/Ø/g" "$REPLY"
sed -i "s/Ï/¥/g" "$REPLY"
sed -i "s/™/Ŧ/g" "$REPLY"
sed -i "s/∏/Þ/g" "$REPLY"
done
```

#### EXTRAIRE LE NOM D'UN FICHIER OU SON EXTENSION

(Voir aussi le paragraphe "Substrings").

```bash
FILE="example.tar.gz"
echo "${FILE%%.*}"
=> example
echo "${FILE%.*}"
=> example.tar
echo "${FILE#*.}"
=> tar.gz
echo "${FILE##*.}"
=> gz
```

#### OPÉRATIONS SUR DES NOMBRES

Pour réaliser des opérations mathématiques en shell, on entoure l'expression d'une double paire de parenthèses.

Le script suivant donne les doubles des chiffres de 1 à 10.

```bash
#!/bin/bash
for i in {1..10}
do
echo $(($i*2))
done
```

Le script suivant additionne 2 nombres entrés par l’utilisateur.

```bash
#!/bin/bash
echo "Enter two numbers:"
read num1 num2
sum=$((num1 + num2))
echo "The sum is $sum."
```

#### OPÉRATIONS SUR LES DATES

Pour écrire la date courante (utile dans des fichiers logs) :

```bash
#!/bin/bash
echo `date`
```

Ou bien :

```bash
#!/bin/bash
echo `date +%c`
```

#### OPÉRATIONS SUR DES FICHIERS

Le script suivant teste le type de l'argument donné en paramètre.

```bash
#!/bin/bash
arg=$1
if [[ -d $arg ]]; then
echo 'Dossier !'
elif [[ -f $arg ]]; then
echo 'Fichier !'
else
echo 'Argument invalide !'
exit 1
fi
```

Le script suivant grep sur le contenu d’un fichier et exporte ce fichier vers un fichier au nom semblable (2008-QTR1.tsv > 2008Q1.txt).

```bash
for f in *-QTR*.tsv do
grep 8-K < "$f" > "${f:0:4}"Q"${f:8:1}".txt
done
```

Le script suivant renomme une série de fichiers \*.gf à partir d’une liste de noms contenus dans le fichier list.txt.

```bash
mapfile -t list < list.txt
i=0
for f in *.gf; do
echo mv "$f" "${list[i++]}_$f"
done
```

#### OPÉRATIONS SUR LES SCRIPTS

Durée d'exécution d'un script :

```bash
time my_script.sh

real    2m5.034s # <-- Actual time taken from start to finish.
user    0m0.000s # <-- CPU time user-space.
sys     0m0.003s # <-- CPU time kernel-space.
```

#### TESTS SUR DES PROCESS

Le script suivant teste si le process 'fetchmail' existe, et si non le lance.

```bash
if [ -z `pidof fetchmail` ] then
echo 'Fetchmail starting…'
fetchmail -av -d 15
fi
```

Commentaire :
'-z' signifie 'chaîne de longueur nulle'.

#### CRÉER DES DOSSIERS EN SÉRIE

Le script suivant crée une série de dossiers dont le nom est construit à partir d'une boucle for.

```bash
for i in {15..52} do
mkdir L13"$i"F
done
```

Le script suivant boucle sur un array, vérifie que chaque item existe, sinon crée le dossier.

```bash
#!/bin/bash
array=( /apache /apache/bin /apache/conf /apache/lib )
for dir in "${array[@]}"
do
[[ ! -d "$dir" ]] && mkdir "$dir"
done
```

#### MAPFILE TO READ INPUT LINES

Le script suivant lit une par une des lignes de données et en utilise les champs :

```bash
lines='John Smith
James Johnson'

mapfile -t lines <<< "$lines"
for line in "${lines[@]}"; do
read name surname <<< "$line"
read -p "Do you want to echo name? surname otherwise "
if [[ $REPLY == "y" ]]; then
echo $name
else
echo $surname
fi
done
```

If lines comes from the output of some command, you could similarly use mapfile -t lines < <(somecommand) directly.

#### REDIRECTIONS

Tableau synthétique. Pour les descriptions détaillées de chaque redirection, voir à la page "Commandes".

```
          || visible in terminal ||   visible in file   || existing
  Syntax  ||  StdOut  |  StdErr  ||  StdOut  |  StdErr  ||   file
==========++==========+==========++==========+==========++===========
    >     ||    no    |   yes    ||   yes    |    no    || overwrite
    >>    ||    no    |   yes    ||   yes    |    no    ||  append
          ||          |          ||          |          ||
   2>     ||   yes    |    no    ||    no    |   yes    || overwrite
   2>>    ||   yes    |    no    ||    no    |   yes    ||  append
          ||          |          ||          |          ||
   &>     ||    no    |    no    ||   yes    |   yes    || overwrite
   &>>    ||    no    |    no    ||   yes    |   yes    ||  append
          ||          |          ||          |          ||
 | tee    ||   yes    |   yes    ||   yes    |    no    || overwrite
 | tee -a ||   yes    |   yes    ||   yes    |    no    ||  append
          ||          |          ||          |          ||
 n.e. (*) ||   yes    |   yes    ||    no    |   yes    || overwrite
 n.e. (*) ||   yes    |   yes    ||    no    |   yes    ||  append
          ||          |          ||          |          ||
|& tee    ||   yes    |   yes    ||   yes    |   yes    || overwrite
|& tee -a ||   yes    |   yes    ||   yes    |   yes    ||  append
```
