# Commandes Unix

#### SYSTÈME

Met à jour le gestionnaire de paquets.

```bash
apt-get update
```

Met à jour les paquets à leur dernière version.

```bash
apt-get upgrade
```

Upgrade operating system to latest release.

```bash
do-release-upgrade
```

Version du système.

```bash
cat /etc/issue
```

Heure du système.

```bash
hwclock –show
```

Donne le statut du démon de synchronisation de l'heure locale à un serveur distant. Pour indiquer le serveur à connecter, éditer le fichier /etc/systemd/timesyncd.conf (ajouter les serveurs, séparés par des espaces, sur la ligne "NTP=" décommentée).

```bash
systemctl status systemd-timesyncd.service
```

Se logger comme root.

```bash
su
```

Se logger comme root avec l'environnement de root, y compris la variable PATH.

```bash
su -
```

Changer mon pw.

```bash
passwd
```

Changer le pw d'un autre utilisateur. Se mettre en su pour éviter les contraintes de longueur et complexité.

```bash
passwd USERNAME
```

Version du kernel.

```bash
apt policy linux-generic
```

Nom du kernel.

```bash
uname -s
```

Version du kernel.

```bash
uname -r
```

Purge les vieux kernels (s'appuie sur byobu qui doit être installé).

```bash
purge-old-kernels
```

Voir tous les kernels installés.

```bash
dpkg -l linux-image-\* | grep ^ii
```

Liste tous les drivers PCI, et indique les modules kernels capables de s'en occuper.

```bash
lspci -k
```

Informations sur l'architecture CPU.

```bash
lscpu
```

Updates the current state of the users user-dirs.dir. Utile pour restaurer les icônes par défaut (Pictures, Music…) si on a remplacé les folders initiaux par des liens symboliques vers un disque externe. Éditer ensuite ~/.config/user-dirs.dirs

```bash
xdg-user-dirs-update
```

Éditeur graphique de gsettings et dconf.

```bash
dconf-editor
```

RAM - Display amount of free and used memory in the system.

```bash
free
```

Éditer ce fichier pour modifier les dossiers systèmes de l'utilisateur (donc aussi les vignettes).

```bash
~/.config/user-dirs.dirs
```

#### MATÉRIEL

Informations on the current settings (keyboard, pointer, screen…).

```bash
xset -q
```

Vitesse de répétition des touches du clavier.

```bash
xset r rate 250 70
```

Lister et modifier les réglages concernant le matériel d'input (souris, clavier).

```bash
xinput
```

Propriétés du device 10 (indice à prendre dans la liste, cf. ligne précédente).

```bash
xinput list-props 10
```

#### CRONTAB

Voir mon fichier crontab.

```bash
crontab -l
```

Éditer mon fichier crontab.

```bash
crontab -e
```

Syntaxe d'une ligne de crontab.

```bash
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  *  <user_command_to_be_executed>
```

Exécuter tâche le 13 janvier et tous les vendredis.

```bash
0 0 13 1 5 tâche
```

Afficher un message toutes les 5 minutes entre 9h et 17h.

```bash
*/5 9-17 * * mon,tue,wed,thu,fri wall "Où en es tu ?"
```

Exécution d'une commande toutes les 6 heures.

```bash
00 */6 * * * /root/scripts/synchronisation-ftp.sh
```

Exécution d'une commande à 22h et écriture dans un fichier log.

```bash
00 22 * * * /root/scripts/sauvegarde.sh >> sauvegarde.log
```

#### WATCH

Exécute <command> toutes les 5 secondes et affiche le résultat rafraîchi dans le Terminal.

```bash
watch -n 5 <command>
```

#### RÉSEAU

Infos sur le réseau et les interfaces (voir man ip pour les objets disponibles).

```bash
ip [OBJECT]
```

Adresses, avec stats, couleurs et option 'human'.

```bash
ip -s -c -h a
```

Renvoie l'adresse IP de ce poste.

```bash
hostname -I
```

Scanne les ports ouverts sur une machine du réseau.

```bash
nmap 192.168.1.11
```

Scanne le réseau local.

```bash
netdiscover -r 192.168.1.0/24
```

Idem.

```bash
avahi-browse --all --terminate
```

Résout l'adresse IP passée en paramètre.

```bash
avahi-resolve-address 192.168.1.254
```

Command-line tool for controlling NetworkManager. Use "nmcli device show" to get complete information about known devices and "nmcli connection show" to get an overview on active connection profiles.

```bash
nmcli
```

#### DEVICES/MOUNT

Espace libre/utilisé sur disque.

```bash
df -h
```

Idem, mais seulement les «/dev/sd\*».

```bash
df -h | grep "^/dev/sd*" | sort
```

(sudo required) Liste des devices (indique notamment la partition de boot).

```bash
fdisk -l
```

Monter /dev/sda1 sur /mnt (type ntfs, pour usb: usually vfat, pour linux: usually ext).

```bash
mount -t ntfs /dev/sda1  /mnt/
```

Idem by verbose path (other names: /dev/disk/by-uuid/, /dev/disk/by-path/, /dev/disk/by-label/).

```bash
mount /dev/disk/by-id/ata-ST31500341AS_9VS2AM04-part1 /some/dir
```

Montage avec --bind: The directories and files in the bind mount are the same as the original. Any modification on one side is immediately reflected on the other side, since the two views show the same data.

```bash
mount --bind /some/where /else/where
```

Vue générale des filesystems et partitions (permet de voir les types, uuid etc.). Voir les available columns en tapant 'lsblk --help'.

```bash
lsblk --fs -l -o NAME,FSTYPE,LABEL,UUID,MOUNTPOINT,SIZE
```

Locate/print block device attributes.

```bash
blkid
```

Trouver un filesystem.

```bash
findmnt
```

-l: list; -o: options list; -t: filter.

```bash
findmnt -lo source,target,fstype,label,options,used -t ext4
```

Liste les devices usb.

```bash
lsusb
```

Liste les partitions.

```bash
cat /proc/partitions
```

Lists information about all available or the specified block devices.

```bash
lsblk -io KNAME,TYPE,SIZE,MODEL
```

#### MODULES

Charger un module kernel.

```bash
modprobe
```

Handle dependency descrs for loadable kernel modules.

```bash
depmod
```

Install loadable kernel module.

```bash
insmod
```

List loaded modules. /proc/modules shows what kernel modules (drivers) are currently loaded into memory.

```bash
lsmod
```

Display information about a kernel module.

```bash
modinfo
```

High level handling of loadable modules.

```bash
modprobe
```

Unload loadable modules.

```bash
rmmod
```

#### SERVICES/DÉMONS

Liste tous les services (systemctl) et leur état.

```bash
systemctl list-unit-files *service
```

Liste tous les services dont le statut est “running”.

```bash
systemctl | grep running
```

Liste tous les services dont le statut est “enabled”.

```bash
systemctl list-unit-files --state=enabled
```

Runs all init scripts with the status command.

```bash
service --status-all
```

Liste tous les démons.

```bash
ls /etc/init.d
```

Pour ajouter le service sdcard.service.

```bash
cd /etc/systemd/system chmod a+x sdcard.service  systemctl --system daemon-reload  systemctl start sdcard.service
```

Pour supprimer le service sdcard.service.

```bash
systemctl stop sdcard.service systemctl disable sdcard.service rm sdcard.service rm sdcard.service systemctl daemon-reload  systemctl reset-failed
```

#### ACCÈS

Pour voir dans quels groupes je suis inscrit.

```bash
id
```

Idem (plus simple).

```bash
groups
```

To know who I am.

```bash
whoami
```

#### PAQUETS

Installer un paquet deb.

```bash
dpkg -i *.deb
```

Désinstaller une application installée avec un paquet deb (faire un dpkg --list + grep pour faire la liste et rechercher le nom exact de l'application).

```bash
dpkg --purge my_app
```

Liste des paquets présents dans la database dpkg.

```bash
dpkg-query -l
```

Liste des paquets installés.

```bash
apt list --installed
```

Liste des paquets à moitié installés.

```bash
dpkg-query -f '${status} ${package}\n' -W | awk '$3 == "half-configured" {print $4}'
```

Idem.

```bash
dpkg-query -f '${status} ${package}\n' -W | awk '$3 != "installed" {print $4}'
```

Idem avec aptitude.

```bash
aptitude search '~i!~M'
```

Cherche un paquet contenant 'word'.

```bash
apt-file search 'word'
```

Supprime les paquets installés en raison de dépendances, et devenus inutiles.

```bash
apt-get autoremove
```

Supprimer les anciens noyaux.

```bash
apt-get autoremove --purge
```

Supprimer le paquet, ses dépendances et ses fichiers de configuration.

```bash
apt-get autoremove --purge
```

Installe nouvelles versions de tous les paquets.

```bash
apt-get upgrade
```

Mise à jour des paquets, se charge de gérer les changements de paquets dépendants (cf. aptitude full-upgrade).

```bash
apt-get dist-upgrade
```

Full update.

```bash
apt-get update; apt-get upgrade; apt-get dist-upgrade; apt autoremove
```

Checks for broken dependencies.

```bash
apt-get check
```

Met à jour la liste des paquets disponibles.

```bash
aptitude update
```

Met à jour les paquets installés vers leurs versions les plus récentes, sans détruire l'existant.

```bash
aptitude safe-upgrade
```

Idem mais installe et supprime selon les besoins.

```bash
aptitude full-upgrade
```

Supprime du cache les paquets qui ne peuvent plus être downloadés.

```bash
aptitude autoclean
```

Liste les paquets orphelins.

```bash
deborphan
```

#### REPOSITORIES

Ajoute un repository dans /etc/apt/sources.list.d/.

```bash
add-apt-repository ppa:/
```

Supprimer un repository.

```bash
add-apt-repository --remove ppa:virtualbox
```

#### KEYS

Ajouter une clé.

```bash
wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | apt-key add -
```

Ajouter une clé dans un autre dossier que /etc/apt/.

```bash
wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | apt-key --keyring /tmp/test add -
```

Liste des clés.

```bash
apt-key list
```

Supprimer une clé.

```bash
apt-key del B9F8
```

#### CD

Commande en argument de cd.

```bash
cd "$(find -L . -name 'xxx' -type d)"
```

#### CP

Copie de plusieurs fichiers contenus dans le dossier test.

```bash
cp ./test/{file1,file2,file3} ./dest
```

#### CHMOD

Droits rw à tout le monde de manière récursive.

```bash
chmod -R a+rw my_rep
```

Droits x au rép. + sous-rép., et non-x aux fichiers.

```bash
chmod -R u-x,g-x,o-x,u+X,g+X,o+X my_rep
```

#### PROCESSES

Donne le pid du shell courant ('1000' par ex.).

```bash
echo $$
```

Montre que c'est un shell.

```bash
ps --pid 1000
```

Donne le pid du parent.

```bash
ps -o ppid= -p 1000
```

Liste des processes.

```bash
ps -aux
```

Trouver le pid de Firefox.

```bash
ps -aux | grep firefox
```

Idem.

```bash
pgrep firefox
```

Idem.

```bash
pidof firefox
```

Kill le process 2301.

```bash
kill -9 2301
```

Tuer le process de Firefox.

```bash
kill -9 `pidof firefox`
```

Idem (encore faut-il être sûr qu'il n'y a qu'un process firefox).

```bash
pkill firefox
```

Tuer tous les process 'fetchmail' avec awk. On redirige d'abord dans un fichier temporaire la sortie de la commande 'ps -aux | grep fetchmail'. On lance ensuite awk sur chaque ligne de ce fichier : si le 11e champ est égal à "fetchmail", on kill le process qui a pour numéro le 2e champ.

```bash
ps -aux | grep fetchmail > ~/tmp; awk '$11 == "fetchmail" {system("kill -9 " $2)}' ~/tmp; rm ~/tmp
```

Displays Linux processes (dynamique). Press shift+m after running the top command or you can interactively choose which column to sort on press Shift+f to enter the interactive menu press the up or down arrow until the %MEM choice is highlighted press s to select %MEM choice press enter to save your selection press q to exit the interactive menu

```bash
top
```

Idem (beaucoup mieux).

```bash
htop
```

#### SCRIPTS :

Mettre une commande en background.

```bash
long-running-command [Ctrl+Z] bg
```

If you run the command chain you specify, the invocation ./script.sh 4 will be in the foreground, and the other scripts will be running in the background. Input will be sent to the foreground script. To suspend the foreground script, press CtrlZ. To send the suspended script to the background to continue running, use the bg command. To see the scripts (or more correctly, jobs) you have and the states they're in, use jobs. To bring a specific job to the fore, use fg and its number (as reported by the aforementioned jobs) with a % prefix, e. g. fg %2. To terminate a specific job, you can either bring it to the foreground with fg and terminate it sanely, or you can kill it, e. g. kill -TERM %2.

```bash
./script.sh 1 &
./script.sh 2 &
./script.sh 3 &
./script.sh 4
```

Exécute script.sh et affiche le temps d'exécution.

```bash
time ./script.sh
```

#### PROGRAMMES :

Lister les dépendances requises par un programme.

```bash
ldd /usr/bin/program
```

#### CHAÎNES

Affiche les nombres de 1 à 10 + point + retour à la ligne.

```bash
printf "%s.\n" $(seq 10)
```

#### FIND

printf = formate le résultat (voir man find pour la liste des balises). %p = nom, %s = taille du fichier.

```bash
find . type f -printf '%p %s'
```

Follow symbolic links.

```bash
find -L
```

Delete every find which doesn't contain 'light'.

```bash
find . ! -name '*light*' -delete
```

Combiner find et grep (fichier modifié il y a moins de 3 minutes et contenant 'rc').

```bash
find ~ -type f -mmin -3 -exec grep -Fl 'rc' {} \;
```

Afficher les dossiers contenant à la fois un fichier "wsgi.py" et un fichier "settings.py" ('execdir': exécute une commande sur le dossier contenant le fichier trouvé, '%h' = folder).

```bash
find . -type f -name wsgi.py -execdir test -f settings.py \; -printf '%h\n'
```

Rename files with find.

```bash
find . -name 'file_*' -exec rename 's/file_/mywish_/' {} \;
```

Rename files with find: replace spaces with underscores.

```bash
find -name '* *' -exec bash -c 'mv "$1" "${1// /_}"' -- {} \;
```

Remove files of a dir, except 'file.txt'.

```bash
find . ! -name 'file.txt' -type f -exec rm -f {} +
```

#### GREP

Inclut seulement les fichiers tex et cherche le mot 'word' (-r = recursive, -n = affiche les numéros de lignes).

```bash
grep -rn --include=*.php word
```

Exclut les fichiers .c et .h.

```bash
--exclude=\*.{c,h}
```

Exclut des répertoires.

```bash
--exclude-dir={dir1,*.dst}
```

Ne retourne que les noms de fichiers.

```bash
-l (L minuscule)
```

Retourne les lignes qui ne matchent pas 'pattern'.

```bash
grep -v 'pattern'
```

Retourne les lignes qui matchent exactement 'pattern' (pas 'pattern1', 'pattern2' etc.).

```bash
grep -w 'pattern'
```

#### NOMBRES

Écrit les nombres de 1 à 10 en colonne.

```bash
seq 1 10
```

Écrit les nombres de 1 à 6 avec 4 caractères ('0001' etc.).

```bash
seq -f "%02g" 6
```

Écrit les nombres de 1 à 10 en colonne.

```bash
jot 10
```

#### DOSSIERS

Créer un dossier et y aller.

```bash
mkdir dossier && cd $_
```

Taille d'un dossier et de ses sous-dossiers.

```bash
du -hsc *
```

Idem.
-h : human-readable
--max-depth=2 : taille des sous-dossiers avec une profondeur de 2.
-hr : trier ('sort') par le champ 'human-readable' ('h') en sens inverse ('r', les plus lourds d'abord)

```bash
du -h --max-depth=2 . | sort -hr
```

Taille d'un dossier et de ses sous-dossiers + interface (basé sur le précédent). Défaut : répertoire courant.

```bash
ncdu [path]
```

Visualiser l'arborescence d'un dossier avec une profondeur de 2.

```bash
tree -L 2
```

List current directory contents.

```bash
dir
```

#### FICHIERS

Permission sur les fichiers et les dossiers. Ainsi 744 équivaut à "rwxr--r--", soit "lecture écriture exécution" pour "u" (user) et "lecture seule" pour "g" (group) et "o" (others). Pour les dossiers : Read (r) : The ability to read the names of files stored in this directory. Write (w) : The ability to rename files in the directory, create new files, or delete existing files, if you also have Execute permissions. If you don't have execute perms, then write perms are meaningless. Execute (x) : The ability to cd into this directory, and access the files in this directory.

```bash
Number    Permission            Symbol
0         No Permission         ---
1         Execute               --x
2         Write                 -w-
3         Execute + Write       -wx
4         Read                  r--
5         Read + Execute        r-x
6         Read + Write          rw-
7         Read + Write +Execute rwx
```

Renvoie les caractéristiques du fichier.

```bash
file path/to/file
```

Trie des fichiers.

```bash
| sort
```

Localise le fichier passé en paramètre.

```bash
locate file_name.ext
```

Ouvre le pdf passé en paramètre.

```bash
evince `locate file.pdf`
```

Lire un fichier en hexadécimal (ou autre format, voire options dans man).

```bash
od -t x name_of_file
```

Extrait le nom du dossier parent de $file.

```bash
dirname $file
```

Coupe un fichier en 8.

```bash
split -n 8 [FILE_NAME]
```

Retourne '1234' (efface les caractères [a-z]). Attention au pathname expansion (cf. man bash): pour l'éviter on peut mettre a-z sans les crochets.

```bash
echo 'abcd1234' | tr -d [a-z]
```

Retourne 'zbcy1294'.

```bash
echo 'abcd1234' | tr ad3 zy9
```

Supprime (squeeze) les espaces en trop dans data.

```bash
tr -s ' ' < data
```

Dispose un fichier en colonnes : No.|Country|Yes/No 01|India|Y 02|US|Y 03|Australia|Y => No. Country Yes/No 01 India Y 02 US Y 03 Australia Y

```bash
column test.txt -t -s "|"
```

Créer les fichiers 'file1', 'file2'… 'file10'.

```bash
touch $(seq -f "file%g" 1 10)
```

Dispose les nombres de 1 à 36 sur 4 lignes et 9 colonnes.

```bash
jot 36 | rs 4 9
```

Renvoie le nombre de termes différents dans le champ 3 (les 2 premiers champs sont sautés) du fichier FileName. (uniq: report or omit repeated lines)

```bash
uniq -f2 FileName | wc -l
```

La même chose si les entrées ne sont pas pré-triées.

```bash
sort -uk3 FileName | wc -l
```

Merge plusieurs pdf dans out.pdf.

```bash
pdfunite file1.pdf file2.pdf [fileN.pdf] out.pdf
```

Extrait du fichier /etc/passwd le 1er field, en utilisant comme délimiteur le caractère ':'

```bash
cut -d: -f1 < /etc/passwd
```

Autre exemple.

```bash
$ s='one_two_three_four_five' $ A="$(cut -d'_' -f2 <<<"$s")" $ echo "$A" two $ B="$(cut -d'_' -f4 <<<"$s")" $ echo "$B" four
```

Donne les 10 premières ligne de file.

```bash
head -10 file
```

Extrait de file toutes les lignes dont le 2e champ ($2) commence par “L”. Example file = 59 LOUIS 1202 +1 60 FREDDIE 1201 +4 61 FINLAY 1200 -2 62 LEON 1137 +12 63 HARLEY 1132 +6 64 DAVID 1127 -1

```bash
awk '$2 ~ /^L/' file
```

Fusionne plusieurs fichiers en plusieurs colonnes séparées par des tabs :
$ cat state Arunachal Pradesh Assam Andhra Pradesh Bihar Chhattisgrah
$ cat capital Itanagar Dispur Hyderabad Patna Raipur
$ paste number state capital 1 Arunachal Pradesh Itanagar 2 Assam Dispur 3 Andhra Pradesh Hyderabad 4 Bihar Patna 5 Chhattisgrah Raipur

```bash
paste
```

Ajoute le numéro de ligne devant chaque ligne de file1. Il faut rediriger vers un autre fichier (file2) sous peine de se retrouver avec un fichier vide.

```bash
nl file1 > file2
```

#### INSTALLATION FROM SOURCES

1. Configure the software
   The configure script is responsible for getting ready to build the software on your specific system. It makes sure all of the dependencies for the rest of the build and install process are available, and finds out whatever it needs to know to use those dependencies.
   Unix programs are often written in C, so we'll usually need a C compiler to build them. In these cases the configure script will establish that your system does indeed have a C compiler, and find out what it's called and where to find it.
2. Build the software
   Once configure has done its job, we can invoke make to build the software. This runs a series of tasks defined in a Makefile to build the finished program from its source code.
   The tarball you download usually doesn't include a finished Makefile. Instead it comes with a template called Makefile.in and the configure script produces a customised Makefile specific to your system.3. Install the software
   Now that the software is built and ready to run, the files can be copied to their final destinations. The make install command will copy the built program, and its libraries and documentation, to the correct locations.
   This usually means that the program's binary will be copied to a directory on your PATH, the program's manual page will be copied to a directory on your MANPATH, and any other files it depends on will be safely stored in the appropriate place.
   Since the install step is also defined in the Makefile, where the software is installed can change based on options passed to the configure script, or things the configure script discovered about your system.
   Depending on where the software is being installed, you might need escalated permissions for this step so you can copy files to system directories. Using will often do the trick.

```bash
./configure
make
make install
```

#### REDIRECTIONS OF STANDARD TO FILES

The standard output stream will be redirected to the file only, it will not be visible in the terminal. If the file already exists, it gets overwritten.

```bash
command > output.txt
```

The standard output stream will be redirected to the file only, it will not be visible in the terminal. If the file already exists, the new data will get appended to the end of the file.

```bash
command >> output.txt
```

The standard error stream will be redirected to the file only, it will not be visible in the terminal. If the file already exists, it gets overwritten.

```bash
command 2> output.txt
```

The standard error stream will be redirected to the file only, it will not be visible in the terminal. If the file already exists, the new data will get appended to the end of the file.

```bash
command 2>> output.txt
```

Both the standard output and standard error stream will be redirected to the file only, nothing will be visible in the terminal. If the file already exists, it gets overwritten.

```bash
command &> output.txt
```

Both the standard output and standard error stream will be redirected to the file only, nothing will be visible in the terminal. If the file already exists, the new data will get appended to the end of the file.

```bash
command &>> output.txt
```

The standard output stream will be copied to the file, it will still be visible in the terminal. If the file already exists, it gets overwritten.

```bash
command | tee output.txt
```

The standard output stream will be copied to the file, it will still be visible in the terminal. If the file already exists, the new data will get appended to the end of the file.

```bash
command | tee -a output.txt
```

Bash has no shorthand syntax that allows piping only StdErr to a second command, which would be needed here in combination with tee again to complete the table. If you really need something like that, please look at "How to pipe stderr, and not stdout?" on Stack Overflow for some ways how this can be done e.g. by swapping streams or using process substitution.

```bash
(*)
```

Both the standard output and standard error streams will be copied to the file while still being visible in the terminal. If the file already exists, it gets overwritten.

```bash
command |& tee output.txt
```

Both the standard output and standard error streams will be copied to the file while still being visible in the terminal. If the file already exists, the new data will get appended to the end of the file.

```bash
command |& tee -a output.txt
```

#### SONS

Convertit 'fichier.wma' en 'fichier.mp3'.

```bash
soundconverter -b -m audio/mpeg -s .mp3 fichier.wma
```

#### IMAGES

Ouvre l'image.

```bash
display image.jpg
```

Crée une vignette à partir de image.jpg (largeur 120).

```bash
convert -thumbnail 120x image.jpg image_thumb.jpg
```

Crée une vignette à partir de image.jpg (hauteur 120).

```bash
convert -thumbnail x120 image.jpg image_thumb.jpg
```

Convertit un jpeg en tiff.

```bash
convert image.jpg image.tiff
```

Convertit une image en CMJN.

```bash
convert image.jpg -colorspace CMYK image_cmyk.jpg
```

Convertit une image en 300 dpi.

```bash
convert image.tiff -density 300 image_300.tiff
```

Détails sur une image.

```bash
identify -verbose image.jpg
```

#### XARGS

Passe en paramètre à la commande 'echo' les arguments donnés en entrée standard.

```bash
cut -d: -f1 < /etc/passwd | sort | xargs echo
```

Processes parallèles.

```bash
echo {1..9} | xargs -n2 -P2 --process-slot-var=index sh -c 'echo "$index" "$@" "$$"' _
```

#### INTERNET

Renvoie l'adresse IP de site.com.

```bash
nslookup site.com
```

Idem.

```bash
dig site.com
```

Idem.

```bash
host site.com
```

Idem.

```bash
getent hosts site.com
```

#### LS

a = all (y compris fichiers cachés), l = list, h = human readable

```bash
ls -alh
```

Liste tous les fichiers .py du dossier courant (on peut utiliser une regex, like [0-9]\*, etc.).

```bash
ls *.py
```

#### CONNEXION & COPIE À DISTANCE

Secure shell distant.

```bash
ssh user@192.168.1.xxx
```

Liste de toutes les connexions ssh en cours.

```bash
netstat -tnpa | grep 'ESTABLISHED.*ssh'
```

Copie file vers remote_file.

```bash
rsync -av /home/user/Desktop/file user@191.168.1.21:/home/user/Desktop/remote_file
```

Idem.

```bash
scp src dest
```

#### DATE

Date.

```bash
date
```

#### TIPS & TRICKS

Équivalent de 'touch foo bar && ls -l bar'

```bash
touch foo bar && ls -l !#:2
```

#### ARCHIVES

Unzip a file [to a particular dest. folder].

```bash
unzip file.zip [-d destination_folder]
```

Créer une archive tar.

```bash
tar -cvf nom_archive nom_dossier
```

Voir contenu sans extraire.

```bash
tar -tf nom_archive.tar
```

Extraire le contenu de l'archive.

```bash
tar -xvf nom_archive.tar
```

Idem vers un autre répertoire.

```bash
tar xzvf nom_archive.tar.gz  -C repertoire
```

Compresser un tar en .tar.gz (plus courant).

```bash
gzip nom_archive.tar
```

Compresser un tar en bz2 (plus puissant et plus lent).

```bash
bzip2 nom_archive.tar
```

Décompresser un .tar.gz.

```bash
gunzip nom_archive.tar.gz
```

Décompresser un .tar.bz2.

```bash
bunzip2 nom_archive.tar.bz2
```

Créer une archive tar + compresser automatiquement en gz.

```bash
tar -zcvf nom_archive.tar.gz nom_dossier
```

Créer une archive tar + compresser automatiquement en bz2.

```bash
tar -jcvf nom_archive.tar.bz2 nom_dossier
```

Extraire le contenu de l'archive après avoir décompressé automatiquement (.gz).

```bash
tar -zxvf nom_archive.tar.gz
```

Extraire le contenu de l'archive après avoir décompressé automatiquement (.bz2).

```bash
tar -jxvf nom_archive.tar.bz2
```

#### ENVIRONNEMENT

Liste des variables d'environnement.

```bash
env
```

````bash
VARNAME="my value"
```bash
À mettre dans .bashrc [/etc/environment] pour ajouter VARNAME à toutes les futures sessions bash [system wide] de manière permanente.

#### ASPIRATEUR DE SITES

```bash
httrack https://site.com -r2 -P 192.168.1.250
```bash
Aspire le site https://site.com, avec une profondeur de 2 et en passant par le proxy.
````
