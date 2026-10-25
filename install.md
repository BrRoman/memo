# Installation Debian

#### Préparer la clé bootable

Télécharger l'image iso de Debian (small image), et se placer dans le répertoire contenant cette image.
Insérer la clé USB, puis chercher son identifiant (en tant que su):

```bash
fdisk -l
=> /dev/sdd1    *   1   487 3911796 6   FAT16
```

On voit que notre clé USB est montée sur /dev/sdd.
Lancer la copie des fichiers :

```bash
dd if=image.iso of=/dev/sdd bs=4M && sync
```

Il ne reste plus qu'à booter sur la clé USB et à se laisser guider.
Partitions :

- /sda1: point de montage: "/". Ne pas oublier de cocher 'bootable'. Label = "SYSTEM".
- /sda2: point de montage: "/media/frromain/DATA".
- /sda3 (peut être changé après avec GParted?): pas plus de 8Go.
  Debian 10 : Quand l'installateur demande s'il faut installer le paquet iwlwifi à partir d'un volume externe, dire non. Ce paquet sera installé après coup (cf. ci-dessous).

#### Installer le wifi

Il faut d'abord ajouter l'option "non free" à la ligne "deb.debian.org" du fichier /etc/apt/sources.list :

deb http://deb.debian.org/debian/ buster main contrib non-free

Ensuite installer le paquet :

```bash
apt-get install firmware-iwlwifi
```

#### Montage de la partition DATA au démarrage

Ajouter la ligne suivante dans /etc/fstab :

```bash
# /media/frromain/DATA was on /dev/sda2 during installation

UUID=9cbb4ca5-7d85-4267-b759-4446a8d4a1f4 /media/frromain/DATA ext4    defaults        0       2
```

#### Paquets

aptitude, curl, tree, htop, ncdu, rsync, locate, imagemagick et imagemagick-doc, barcode, qrencode, rename (pour renommer des fichiers en série), aspell-fr, mplayer, calcurse (ou taskwarrior), units

#### Configurer le réseau

#### Echanges

smb://tm-rs820rp-01.local/data/Echanges

#### Mac imprimerie

sftp://mac-mini-imprimerie.local/Users/perebenoit-joseph

#### Default applications

**Mails**: Thunderbird

**Calendar**: Thunderbird

**Music**: Videos

**Videos**: VLC

#### Raccourcis clavier dans Settings

**Home folder**: Alt+Super+N

**Terminal**: Alt+Super+T

**Text Edit** `(gnome-text-editor --new-window)`: Alt+Super+E

**Email**: Alt+Super+M

**Browser**: Alt+Super+B

**Calculator**: Alt+Super+K

**Settings**: Alt+Super+S

**Screenshot** (Debian 10 seulement) (gnome-screenshot -i): (Alt+Super+O)

**Switch windows of an application**: Ctrl+Tab

**Screen reader**: disabled

**Rhythmbox** `(rhythmbox)`: Alt+Super+R

**Anki** `(anki)`: Alt+Super+A

**Golden Dict** `(goldendict)`: Alt+Super+G

**Virtual Box** `(virtualbox)`: Alt+Super+V

**Collatinus (Debian 10 seulement)** `(collatinus)`: Alt+Super+C

**XnView** `(xnview)`: Alt+Super+X

**VSCode** `(code)`: Alt+Super+C

**Utiles** `(libreoffice /media/frromain/DATA/Documents/Divers/Utiles.ods)`: Alt+Super+U

**Zorell hébreu**` (evince /home/frromain/Documents/Sainte\ Écriture/Langues\ bibliques/Hébreu/Zorell\ hébreu.pdf)`: Alt+Super+Z

**Zorell grec** `(evince /home/frromain/Documents/Sainte\ Écriture/Langues\ bibliques/Grec/NT/Zorell\ grec.pdf)`: Alt+Super+Shift+Z

**Bailly** `(evince /home/frromain/Documents/Langues/Grec/bailly-2020-hugo-chavez-20200718.pdf)`: Alt+Super+H

**Jastrow** `(evince /home/frromain/Documents/Sainte\ Écriture/Langues\ bibliques/Araméen/Jastrow-AramaicHebrew.pdf)`: Alt+Super+W

**Syriaque** `(evince /home/frromain/Documents/Sainte\ Écriture/Langues\ bibliques/Syriaque/epdf.pub_a-compendious-syriac-dictionary.pdf)`: Alt+Super+Y

**Gaffiot** `(evince /home/frromain/Documents/Langues/Latin/Dictionnaires\ latins/Gaffiot_2016.pdf)`: Alt+Super+F

**Vetus Latina** `(evince /home/frromain/Documents/Sainte\ Écriture/0\ Bibles/Vetus\ Latina.pdf)`: Alt+Super+I

**Polyglotte de Walton** `(evince /home/frromain/Documents/Sainte\ Écriture/0\ Bibles/Polyglotte\ Walton/Walton\'s\ Polyglot\ -\ Totum.pdf)`: Alt+Super+P

**PierreQuiBible** (needs package `python3-pyqt5`) `(python3 /home/frromain/Documents/Sainte\ Écriture/Divers/PQV/PierreQuiBible/rollingBible.py)`: Alt+Super+Shift+P

**DevDocs** `(/opt/google/chrome/google-chrome --profile-directory=Default --app-id=ahiigpfcghkbjfcibpojancebdfjmoop)`: Alt+Super+D

#### Suppression de la mise en veille automatique (typiquement dans le cas d'un serveur)

Il suffit de modifer (en tant que su) le fichier /etc/systemd/sleep.conf :

```bash
    AllowSuspend=no
    AllowHibernation=no
    AllowSuspendThenHibernate=no
    AllowHybridSleep=no
```

#### Clés SSH

```bash
ssh-keygen
```

```bash
ssh-copy-id username@remote
```

#### Firefox

Bookmarks, home, proxy, sécurité, dossier de téléchargement, xdebug-helper (addon pour débugger PHP).

#### Vim

~/.vimrc :

```bash
    colorscheme default
    set laststatus=2
    set statusline+=%F%r%m%=\ %p%%\ %l:%c
    set number
    set formatoptions+=w
    set textwidth=0
    set tabstop=4
    set softtabstop=4
    set shiftwidth=2
    set expandtab
    set autoindent
    set wrap
    set linebreak
    set breakindent
    set cursorline
    :hi CursorLine cterm=bold
    :map <CR> $a<Del> <Esc>
    :map <C-N> i{}<Esc>hpl%
```

Ne pas oublier d'installer vim-gtk3, afin de pouvoir utiliser le clipboard système.

#### Mails (mutt)

Installer mutt, ripmime, fetchmail, procmail, msmtp et links.
Ensuite copier les fichiers de configuration :

~/.muttrc :

```bash
    alternative_order text/plain text/enriched text/html
    auto_view text/html
    bind index D purge-message
    bind index <C-down> sidebar-next
    bind index <C-right> sidebar-open
    bind index <C-up> sidebar-prev
    color normal default default
    color index brightcolor196 default "~N"
    color index color214 default "~F"
    color indicator black color28
    color status brightcolor196 color18
    folder-hook . set sort=date-sent
    hdr_order Date From To Cc Subject
    ignore *
    macro index,pager S "ripmime -i - -d "
    mailboxes =Inbox =Sent =Postponed =Trash
    set alias_file=~/Mail/.aliases
    set attribution="On %d, %n wrote:"
    set beep
    set beep_new
    set charset=utf-8
    set date_format="%d-%m-%Y %H:%M:%S%p"
    set delete=yes
    set edit_hdrs
    set editor="vim -c 'set spell spelllang=fr'"
    set envelope_from_address="editeur@traditions-monastiques.com"
    set fast_reply
    set folder=~/Mail
    set forward_format="[%a: %s]"
    set from="editeur@traditions-monastiques.com"
    set honor_followup_to
    set hostname="traditions-monastiques.com"
    set include=yes
    set index_format="%Z %D %-25.25L %s %*  |%-2.1X |%-4.4c"
    set ispell="/usr/bin/aspell -d french -e -c"
    set mail_check=5
    set mailcap_path = ~/.mailcap
    set mime_forward=yes
    set pager_index_lines=10
    set pager_stop
    set postponed=~/Mail/Postponed
    set quit=ask-yes
    set realname="Frère Romain Marie"
    set record=~/Mail/Sent
    set reply_to
    set send_charset=utf-8
    set sendmail="/usr/bin/msmtp"
    set sidebar_visible
    set sidebar_width=20
    set sig_on_top
    set signature=~/Mail/.signature
    set spoolfile=~/Mail/Inbox
    set strict_threads
    set text_flowed=no
    set timeout=10
    set trash=~/Mail/Trash
    set use_domain
    set use_envelope_from
    set use_from
    set xterm_set_titles=yes
    set xterm_title="Mutt with %?m?%m messages&no messages?%?n? [%n NEW]?"
    source ~/Mail/.aliases
    unignore From To Cc Subject Date Reply-To
    unset markers
```

~/.fetchmailrc :

```bash
    poll 192.168.1.250
    uidl
    port 995
    protocol POP3
    username editeur@traditions-monastiques.com
    password my_super_password
    mda "/usr/bin/procmail -Y -d %T"
```

~/.procmailrc :

```bash
    MAILDIR=$HOME/Mail
    LOGFILE=$HOME/.procmaillog
    LOGABSTRACT=no
    VERBOSE=off
    DEFAULT=$MAILDIR/Inbox
    ORGMAIL=$MAILDIR/Emergency
    SHELL=/bin/bash

    :0
    * ^From.*
    $DEFAULT
```

~/.msmtprc :

```bash
    account dev
    account default : dev
    user clairval
    host
    port 587
    auth plain
    password <my_super_password>
```

~/.mailcap :

```bash
    # Pour ouvrir des messages au format html avec Firefox (raccourci clavier "m"):
    text/html; firefox '%s' & sleep 10;

    # Ouvrir autres types MIME :
    text/html; links -html-assume-codepage %{charset} '%s'; needsterminal; description=HTML Text; nametemplate=%s.html
    image/jpeg; /usr/bin/eog %s; test=test -n "$DISPLAY"
    image/jpg; /usr/bin/eog %s; test=test -n "$DISPLAY"
    image/png; /usr/bin/eog %s; test=test -n "$DISPLAY"
    image/tiff; /usr/bin/eog %s; test=test -n "$DISPLAY"
    application/pdf; /usr/bin/evince %s; test=test -n "$DISPLAY"
    audio/x-wav; /usr/bin/mplayer %s; test=test -n "$DISPLAY"
```

Enfin lancer fetchmail (en mode nossl) et mutt :

```bash
rm ~/.fetchmail.pid; fetchmail --nosslcertck -av -d 10; mutt
```

#### Taskwarrior

Fichier de config (~/.taskrc) :

```bash
    color=on
    color.alternate=on
    color.uda.priority.H=color9
    color.uda.priority.M=color11
    color.uda.priority.L=color2
    column.padding=3
    default.command=list
    editor=vim
    nag=""
    report.list.columns=id,project,description
    report.list.labels=id,Project,Description
    report.list.sort=priority+\/,project+\/,description+
    verbose=off
```

#### Configurer le proxy pour root

Dans ~/.bashrc :

```bash
export HTTP_PROXY=http://192.168.1.250:8080
```

```bash
export HTTPS_PROXY=http://192.168.1.250:8080
```

Dans ~/.wgetrc :

```bash
HTTP_PROXY=http://192.168.1.250:8080
```

```bash
HTTPS_PROXY=http://192.168.1.250:8080
```

#### Clés

Lors d'une installation, en cas de "clé inexistante", la télécharger (remplacer <KEY> par la clé) :

```bash
apt-key adv --keyserver keyserver.ubuntu.com --keyserver-options http-proxy=http://192.168.1.250:8080 --recv-keys <KEY>
```

#### Virtualbox (Debian 10)

Créer le fichier /etc/apt/sources.list.d/virtualbox.list:

```bash
vim /etc/apt/sources.list.d/virtualbox.list
```

Écrire la ligne suivante, enregistrer et fermer:

```bash
deb https://download.virtualbox.org/virtualbox/debian buster contrib
```

Puis:

```bash
wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | apt-key add -
wget -q https://www.virtualbox.org/download/oracle_vbox.asc -O- | apt-key add -
apt-get update
apt-get install virtualbox-7.0
```

Extensions:

```bash
wget https://download.virtualbox.org/virtualbox/7.0.0/Oracle_VM_VirtualBox_Extension_Pack-7.0.0.vbox-extpack
```

#### Virtualbox (Debian 12)

Installation :

```bash
wget -O- -q https://www.virtualbox.org/download/oracle_vbox_2016.asc | sudo gpg --dearmour -o /usr/share/keyrings/oracle_vbox_2016.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/oracle_vbox_2016.gpg] http://download.virtualbox.org/virtualbox/debian bookworm contrib" | sudo tee /etc/apt/sources.list.d/virtualbox.list
sudo apt update
sudo apt install virtualbox-7.1
```

Extensions :

```bash
sudo apt install virtualbox-7.1
wget https://download.virtualbox.org/virtualbox/7.1.0/Oracle_VirtualBox_Extension_Pack-7.1.0.vbox-extpack
sudo vboxmanage extpack install Oracle_VirtualBox_Extension_Pack-7.1.0.vbox-extpack
```

Vérification des extensions :

```bash
vboxmanage list extpacks
```

Ajouter User to the vboxusers Group :

```bash
sudo usermod -a -G vboxusers frromain
```

Reboot.

Vérifier que User est dans vboxusers :

```bash
groups $USER
```

Uninstall :

```bash
sudo apt purge virtualbox-7.1
sudo apt autoremove
sudo rm /etc/apt/sources.list.d/virtualbox.list
```

#### Apache

Ajouter o+rx sur le dossier /media/frromain.

Dans le fichier /etc/apache2/envvars, changer www-data en frromain aux lignes suivantes, pour que les fichiers écrits par apache aient frromain comme propriétaire :

```bash
    export APACHE_RUN_USER=frromain
    export APACHE_RUN_GROUP=frromain
```

Dupliquer /etc/apache2/sites-available/000-default.conf avant de le modifier ainsi :

```apache
<VirtualHost *:80>
    ServerName localhost
    DocumentRoot /media/frromain/DATA/Sites
    <Directory "/media/frromain/DATA/Sites">
        Options FollowSymLinks
        AllowOverride all
        Require all granted
        Order allow,deny
        Allow from all
    </Directory>

    <Directory "/media/frromain/DATA/Sites/sql">
        Options FollowSymLinks
        AllowOverride all
        Require all granted
        Order deny,allow
        deny from all
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

<VirtualHost *:80>
    ServerName gregorio.local
    DocumentRoot /media/frromain/DATA/Sites/gregorio-doc
    <Directory "/media/frromain/DATA/Sites/gregorio-doc/">
        Options FollowSymLinks
        AllowOverride all
        Require all granted
        Order allow,deny
        Allow from all
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

listen 8000

NameVirtualHost *:8000
<VirtualHost *:8000>
    ServerName localhost
    ServerAdmin editeur@traditions-monastiques.com
    DocumentRoot /home/frromain/Sites/abbaye/abbaye
    WSGIDaemonProcess abbaye.asj.com processes=2 threads=15 display-name=%{GROUP} python-home=/home/frromain/Sites/abbaye python-path=/home/frromain/Sites/abbaye/abbaye
    WSGIProcessGroup abbaye.asj.com
    WSGIScriptAlias / /home/frromain/Sites/abbaye/abbaye/abbaye/wsgi.py
    <Directory /home/frromain/Sites/abbaye/abbaye/abbaye>
        <Files wsgi.py>
            Require all granted
        </Files>
    </Directory>

    Alias /statics/ /home/frromain/Sites/abbaye/abbaye/statics/
    <Directory /home/frromain/Sites/abbaye/abbaye/statics>
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
```

#### MySQL

MySQL8 n'est pas dispo dans les paquets Debian. Il faut l'installer manuellement à partir d'un dépôt distant :

```bash
cd /tmp
```

Debian 10:

```bash
wget https://dev.mysql.com/get/mysql-apt-config_0.8.22-1_all.deb
```

```bash
dpkg -i mysql-apt-config_0.8.22-1_all.deb
```

Debian 12:

```bash
wget https://dev.mysql.com/get/mysql-apt-config_0.8.30-1_all.deb
```

```bash
dpkg -i mysql-apt-config_0.8.30-1_all.deb
```

Puis :

```bash
apt update
```

```bash
apt install mysql-server
```

Attention : bien sélectionner **mysql8.0** ! (pas la version "LTS")

Une fois l'installation achevée, procéder à l'installation sécurisée :

```bash
mysql_secure_installation
```

Créer le fichier ~/.my.cnf (pour ne pas avoir à saisir user et mdp à chaque connexion MySQL) :

```bash
    [mysql]
    user=root
    password=my_super_password
    [mysqldump]
    user=root
    password=my_super_password
```

Enfin dans une console MySQL :

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '<my_password>';
```

#### Python

Installer pip, l'installateur de packages Python :

```bash
aptitude install python3-pip
```

Avec pip installer virtualenv :

```bash
pip3 install virtualenv
```

#### PHP7.3 + modules php7.3-mysql et php7.3-gd (ce dernier pour les statistiques)

#### Importer les bases de données

#### Polices

Fonts (Arno Pro, FlavGaramond, Garamond Premier Pro, GregPlantin, Menlo-Regular.ttf, PlantinStd, SilentiumPro-RomanI.otf) à copier dans /usr/share/fonts (ttf dans truetype et otf dans opentype). Puis :

```bash
fc-cache -fv
```

Ou lancer (en tant que su-) le script `/home/frromain/Scripts/fonts.sh`.

#### LaTeX

Installer texlive texlive-luatex texlive-xetex texlive-lang-european texlive-lang-french texlive-extra-utils.

Ne pas installer texlive-full, qui semble faire des problèmes.

texlive-extra-utils contient latexindent, qui sert pour VSCode (formattage et compilation lors du Ctrl-S).

En cas de "font not found", vider le cache (c'est-à-dire supprimer le dossier ~/.texlive2013/texmf-var/luatex-cache/).

#### Gregorio

Nécessite le paquet cmake.

Télécharger l'archive tar.bz2 sur le site de gregorio (releases).

Désarchiver.

Dans le dossier obtenu, exécuter **(en tant que su)** :

```bash
./build.sh
```

```bash
./install.sh
```

#### Git

Fichier ~/.gitconfig :

```bash
    [user]
    name = BrRoman
    email = editeur@traditions-monastiques.com
    [alias]
    ck = checkout
    ci = commit -m
    st = status -u
    lg = log --graph --date=format:\"%d/%m/%Y %H:%M\" --pretty=format:\"%Cred%h%Creset %C(cyan)%d%Creset %s %C(yellow)%an%Creset %C(green)%ad%Creset\" --decorate --all
    [diff]
    mnemonicprefix = true
    renames = true
    wordRegex = .
    [core]
    editor = vim
    [http]
    proxy = http://192.168.1.250:8080
    [https]
    proxy = http://192.168.1.250:8080
```

Fichier ~/.netrc :

```bash
machine github.com login BrRoman password my_super_password
```

#### Crons

Éditer le crontab :

```bash
crontab -e
```

Lignes à ajouter à la fin du fichier :

```bash
    */20 8-20 * * * /home/frromain/Scripts/server.sh
    30 10,16 * * * /home/frromain/Scripts/github.sh
```

#### VSCode

Clé :

```bash
curl --proxy http://192.168.1.250:8080 https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
```

```bash
install -o root -g root -m 644 microsoft.gpg /etc/apt/trusted.gpg.d/
```

Dépôt :

```bash
echo "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list
```

Installation proprement dite :

```bash
apt update
```

```bash
apt install code
```

Fichier de config (~/.config/Code/User/settings.json) :

```json
{
  // Editor:
  "editor.foldingHighlight": true,
  "editor.fontFamily": "monospace",
  "editor.fontSize": 14,
  "editor.formatOnSave": true,
  "editor.minimap.enabled": false,
  "editor.renderControlCharacters": true,
  "editor.renderWhitespace": "all",
  "editor.smoothScrolling": true,
  "editor.suggest.shareSuggestSelections": true,
  "editor.suggestSelection": "first",
  "editor.tabSize": 4,
  "editor.wordWrap": "on",
  "editor.wrappingIndent": "indent",
  // Explorer:
  "explorer.confirmDelete": false,
  "explorer.confirmDragAndDrop": false,
  "explorer.openEditors.visible": 12,
  // Problems:
  "problems.decorations.enabled": false,
  "window.newWindowDimensions": "maximized",
  "window.openFilesInNewWindow": "on",
  "window.restoreFullscreen": true,
  "window.restoreWindows": "none",
  // Workbench:
  "workbench.activityBar.visible": true,
  "workbench.colorCustomizations": {
    "editor.background": "#000000",
    "editor.foreground": "#ffffff",
    "editor.lineHighlightBorder": "#444444",
    "statusBar.background": "#111111",
    "statusBar.border": "#777777"
  },
  "workbench.editor.closeOnFileDelete": true,
  "workbench.editor.highlightModifiedTabs": true,
  "workbench.editor.openPositioning": "last",
  "workbench.editorAssociations": {
    "*.ipynb": "jupyter-notebook",
    "*.pdf": "default"
  },
  "window.menuBarVisibility": "toggle",
  "workbench.startupEditor": "welcomePage",
  // Files:
  "files.autoSave": "afterDelay",
  "files.autoSaveDelay": 5000,
  // Terminal:
  "terminal.external.linuxExec": "gnome-terminal",
  "terminal.integrated.allowChords": false,
  "terminal.integrated.confirmOnExit": true,
  "terminal.integrated.fontFamily": "monospace",
  "terminal.integrated.fontSize": 13,
  "terminal.integrated.tabs.enabled": true,
  "terminal.integrated.tabs.hideCondition": "never",
  // Debug:
  "debug.allowBreakpointsEverywhere": true,
  // Git:
  "diffEditor.ignoreTrimWhitespace": false,
  "diffEditor.renderSideBySide": true,
  "git.confirmSync": false,
  "git.showProgress": true,
  "git-graph.maxDepthOfRepoSearch": 1,
  // TODO highlight:
  "todo-tree.general.tags": ["FIXME", "TODO", "[ ]", "[x]"],
  "todo-tree.highlights.defaultHighlight": {
    "fontWeight": "bold",
    "foreground": "#FF9600",
    "type": "tag"
  },
  "todo-tree.highlights.customHighlight": {
    "TODO": {
      "icon": "pin",
      "iconColour": "yellow"
    },
    "FIXME": {
      "icon": "alert",
      "iconColour": "red"
    }
  },
  "todo-tree.regex.regex": "(//|#|<!--|;|/\\*|^|^\\s*(-|\\d+.))\\s*($TAGS)",
  // HTML:
  "html.format.templating": true,
  // Javascript:
  "javascript.updateImportsOnFileMove.enabled": "always",
  "[json]": {
    "editor.defaultFormatter": "vscode.json-language-features"
  },
  // LaTeX:
  "latex-workshop.latex.recipes": [
    {
      "name": "lualatex",
      "tools": ["lualatex"]
    }
  ],
  "latex-workshop.latex.tools": [
    {
      "name": "lualatex",
      "command": "lualatex",
      "args": ["--shell-escape", "%DOC%.tex"],
      "env": {}
    }
  ],
  // Python:
  "python.pythonPath": "${workspaceFolder}/bin/python3.7/",
  "python.formatting.autopep8Path": "${workspaceFolder}/bin/autopep8",
  "python.linting.pylintPath": "${workspaceFolder}/bin/pylint",
  "python.linting.enabled": true,
  "python.linting.pylintEnabled": true,
  "python.linting.pylintArgs": ["--load-plugins", "pylint_django"],
  "emmet.includeLanguages": {
    "django-html": "html"
  },
  "files.associations": {
    "*.html": "django-html"
  },
  "notebook.cellToolbarLocation": {
    "default": "right",
    "jupyter-notebook": "left"
  }
}
```

Keybindings (~/.config/Code/User/keybindings.json) :

```json
[
  {
    "key": "ctrl+shift+[KeyM]",
    "command": "workbench.action.openSettingsJson"
  },
  {
    "key": "ctrl+f9",
    "command": "editor.debug.action.goToNextBreakpoint"
  },
  {
    "key": "ctrl+shift+f9",
    "command": "editor.debug.action.goToPreviousBreakpoint"
  }
]
```

Installer les extensions :

```
    Auto close tags
    Auto rename tags
    Beautify
    Bookmarks
    Bracket pair colorizer
    Clipboard History
    CSS peek
    Django
    Git graph
    Highlight matching tags
    Htmltagwrap
    jQuery code snippets
    LaTeX workshop
    Markdown all in one
    PHP debug
    PHP intellisense
    Project manager
    Python
    Remote-ssh
    Todo tree
    Vim
```

#### Collatinus

Installation (paquet) :

```bash
    aptitude install collatinus
```

L'importation des dicos via Collatinus (cf. barre de menu "Options > Télécharger…") ne fonctionne pas. Il faut donc :

1. Installer un paquet supplémentaire :

```bash
    aptitude install qt5-image-formats-plugins
```

2. Copier à la main le contenu du dossier ~/Documents/Langues/Latin/Collatinus/dicos/ dans ~/.local/share/collatinus/data/dicos/ :

#### Anki (pour Debian 12)

Aller dans le répertoire où se trouve l'archive, puis :

```bash
tar xaf anki-24.11-linux-qt6.tar.zst
cd anki-24.11-linux-qt6/
./install.sh
```

#### Autres logiciels

Anki (paquet dans Debian 10 seulement), Audacity, Calibre pour lire les epubs (paquet), Gimp et gimp-help-en (paquets), GoldenDict (paquet), Google chrome (.deb), RawTherapee (paquet), Scribus (paquet), Stellarium (paquet), VLC (paquet), XnView (.deb).

#### Installation sans proxy

Aptitude avec option sans proxy :

```bash
    aptitude -o Acquire::http::proxy=false install <package>
```

#### Installation automatique

Exporter la liste des paquets installés :

```bash
dpkg --get-selections > my_package_list.txt
```

Ou bien, pour exporter uniquement les paquets explicitement installés :

```bash
aptitude search -F '%p %C' '~i!~M' > my_package_list.txt
```

Pour faire une installation à partir de cette lite :

```bash
    apt-cache dumpavail | dpkg --merge-avail
    dpkg --set-selections < my_package_list.txt
    apt-get dselect-upgrade
```
