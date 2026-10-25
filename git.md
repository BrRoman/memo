# Git

Montre les détails d'un commit.

```bash
git show <sha du commit>
```

Après avoir résolu un conflit.

```bash
git commit
```

Pour que Git ne suive plus un dossier, mais que ce dossier reste dans le working directory. Il faut d'abord avoir ajouté ce dossier au .gitignore.

```bash
git rm -r --cached <path to folder>
```

Montre les branches et les remote-trackings.

```bash
git branch -vv
```

Montre les différences entre c1 et c2 sur le fichier mentionné.

```bash
git diff -p <commit_1> <commit_2> <nom_du_fichier>
```

Historique d'un fichier.

```bash
git blame <nom fichier>
```

Blâme sur une ou plusieurs lignes de code.

```bash
git blame -L 150,+11 -- file.sh
```

Changer l’url du remote ‘remote_rep’.

```bash
git remote set-url remote_rep <new_url> [<old_url]
```

Chercher <pattern> dans les logs.

```bash
git log –grep=<patter>
```

Pour merger les modifs de branchA dans la branche courante à l’état unstaged.

```bash
git merge --no-commit --squash branchA git reset HEAD
```

Ne plus tracker le fichier settings.py.

```bash
1. Add it to .gitignore
2. Remove the file from git without deleting the file: git rm --cached path/to/settings.py
3. Commit the change: git commit -m "Remove settings.py"
```

Rapatrier un submodule.

```bash
git submodule update --remote
```
