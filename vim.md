# Vim

Va à la dernière modification.

```bash
'.
```

Idem.

```bash
''
```

Idem.

```bash
'^
```

Compte le nombre d'occurrences de 'pattern'.

```bash
:%s/pattern//ng
```

Affiche toutes les occurrences de 'pattern'. A bit of trivia: This command is the origin of the name 'grep', as it would commonly be described as g/re/p, where re stands for "regular expression".

```bash
:g/pattern/p
```

Efface toutes les lignes.

```bash
:1,$d
```

Efface toutes les lignes (g) qui contiennent ‘pattern’.

```bash
:g/pattern/d
```

Efface toutes les lignes (g) qui ne contiennent pas ‘pattern’.

```bash
:g!/pattern/d
```

Delete to mark a.

```bash
d'a
```

Yank to mark a.

```bash
y'a
```

Quick ref.

```bash
:h quickref
```

Retourne en mode visuel après une commande, avec la même sélection que la dernière fois.

```bash
gv
```

Met la sélection en uppercase.

```bash
U
```

Met la sélection en lowercase.

```bash
u
```

Chemin du fichier courant.

```bash
1^g
```

Format fichier = php + txtfmt (permet d'avoir les 2 highlightings).

```bash
set ft=php.txtfmt
```

Exécuter le fichier courant (%).

```bash
:!./%
```

Exécuter le fichier courant avec bash.

```bash
:!bash %
```
