# Python / Django

##### Les chevrons (>>>) indiquent que la commande doit être exécutée dans un shell Python.

#### THE ZEN OF PYTHON

```py
>>> import this
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
```

#### COMMANDES SYSTÈME

Exécuter une commande système en Python :

```py
>>> import os
>>> os.system('ls -alh')
```

#### SLICES

Inverser une chaîne ([begin:end:step]) :

```py
>>> 'Bonjour’[::-1]
=> 'ruojnoB’
```

#### VARIABLES

Inverser les valeurs de deux variables :

```py
# In-place value swapping

# Let's say we want to swap
# the values of a and b...
a = 23
b = 42

# The "classic" way to do it
# with a temporary variable:
tmp = a
a = b
b = tmp

# Python also lets us
# use this short-hand:
a, b = b, a
```

#### REGEX

Compter le nombre d'occurrences d'un motif dans une chaîne :

```py
>>> import re
>>> len(re.findall('ba', 'babar'))
2
```

#### TESTS

Tests multiples :

```py
# Different ways to test multiple
# flags at once in Python
x, y, z = 0, 1, 0

if x == 1 or y == 1 or z == 1:
print('passed')

if 1 in (x, y, z):
print('passed')

# These only test for truthiness:
if x or y or z:
print('passed')

if any((x, y, z)):
print('passed')
```

Différence entre "is" et "==" :

```py
# "is" vs "=="

>>> a = [1, 2, 3]
>>> b = a

>>> a is b
True
>>> a == b
True

>>> c = list(a)

>>> a == c
True
>>> a is c
False

# • "is" expressions evaluate to True if two
#   variables point to the same object

# • "==" evaluates to True if the objects
#   referred to by the variables are equal
```

#### ITÉRABLES

Trouver le ou les élément(s) le(s) plus commun(s) d'un itérable :

```py
# collections.Counter lets you find the most common
# elements in an iterable:

>>> import collections
>>> c = collections.Counter('helloworld')

>>> c
Counter({'l': 3, 'o': 2, 'e': 1, 'd': 1, 'h': 1, 'r': 1, 'w': 1})

>>> c.most_common(3)
[('l', 3), ('o', 2), ('e', 1)]
```

Afficher toutes les combinaisons possibles d'un itérable :

```py
# itertools.permutations() generates permutations
# for an iterable. Time to brute-force those passwords ;-)

>>> import itertools
>>> for p in itertools.permutations('ABCD'):
...     print(p)

('A', 'B', 'C', 'D')
('A', 'B', 'D', 'C')
('A', 'C', 'B', 'D')
('A', 'C', 'D', 'B')
('A', 'D', 'B', 'C')
('A', 'D', 'C', 'B')
('B', 'A', 'C', 'D')
('B', 'A', 'D', 'C')
('B', 'C', 'A', 'D')
('B', 'C', 'D', 'A')
('B', 'D', 'A', 'C')
('B', 'D', 'C', 'A')
('C', 'A', 'B', 'D')
('C', 'A', 'D', 'B')
('C', 'B', 'A', 'D')
('C', 'B', 'D', 'A')
('C', 'D', 'A', 'B')
('C', 'D', 'B', 'A')
('D', 'A', 'B', 'C')
('D', 'A', 'C', 'B')
('D', 'B', 'A', 'C')
('D', 'B', 'C', 'A')
('D', 'C', 'A', 'B')
('D', 'C', 'B', 'A')
```

#### LISTES

Compréhension de listes :

```py
vals = [expression
for value in collection
if condition]

# This is equivalent to:

vals = []
for value in collection:
    if condition:
        vals.append(expression)

# Example:

>>> even_squares = [x * x for x in range(10) if not x % 2]
>>> even_squares
[0, 4, 16, 36, 64]
```

Cela fonctionne aussi avec une liste de listes :

```py
>>> xss = [
    [1, 2, 3],
    [4, 5, 6],
    [7],
    [8, 9]
]
>>> flat_list = [
    x
    for xs in xss
    for x in xs
]
>>> flat_list
[1, 2, 3, 4, 5, 6, 7, 8, 9]
```

Listes sans indices, utiles pour diverses opérations :

```py
# Python's list slice syntax can be used without indices
# for a few fun and useful things:

# You can clear all elements from a list:
>>> lst = [1, 2, 3, 4, 5]
>>> del lst[:]
>>> lst
[]

# You can replace all elements of a list
# without creating a new list object:
>>> a = lst
>>> lst[:] = [7, 8, 9]
>>> lst
[7, 8, 9]
>>> a
[7, 8, 9]
>>> a is lst
True

# You can also create a (shallow) copy of a list:
>>> b = lst[:]
>>> b
[7, 8, 9]
>>> b is lst
False
```

#### OPÉRATIONS SUR LES DICTIONNAIRES

Affichier un dictionnaire avec le module json :

```py
# The standard string repr for dicts is hard to read:
>>> my_mapping = {'a': 23, 'b': 42, 'c': 0xc0ffee}
>>> my_mapping
{'b': 42, 'c': 12648430. 'a': 23}  # 😞

# The "json" module can do a much better job:
>>> import json
>>> print(json.dumps(my_mapping, indent=4, sort_keys=True))
{
"a": 23,
"b": 42,
"c": 12648430
}

# Note this only works with dicts containing
# primitive types (check out the "pprint" module):
>>> json.dumps({all: 'yup'})
TypeError: keys must be a string
```

Trier un dictionnaire par les values :

```py
# How to sort a Python dict by value
# (== get a representation sorted by value)

>>> xs = {'a': 4, 'b': 3, 'c': 2, 'd': 1}

>>> sorted(xs.items(), key=lambda x: x[1])
[('d', 1), ('c', 2), ('b', 3), ('a', 4)]

# Or:

>>> import operator
>>> sorted(xs.items(), key=operator.itemgetter(1))
[('d', 1), ('c', 2), ('b', 3), ('a', 4)]
```

La méthode get() peut être appliquée à un dict, et on peut lui donner une valeur par défaut :

```py
# The get() method on dicts
# and its "default" argument

name_for_userid = {
382: "Alice",
590: "Bob",
951: "Dilbert",
}

def greeting(userid):
return "Hi %s!" % name_for_userid.get(userid, "there")

>>> greeting(382)
"Hi Alice!"

>>> greeting(333333)
"Hi there!"
```

Merger deux dictionnaires :

```py
# How to merge two dictionaries
# in Python 3.5+

>>> x = {'a': 1, 'b': 2}
>>> y = {'b': 3, 'c': 4}

>>> z = {**x, **y}

>>> z
{'c': 4, 'a': 1, 'b': 3}

# In Python 2.x you could
# use this:
>>> z = dict(x, **y)
>>> z
{'a': 1, 'c': 4, 'b': 3}

# In these examples, Python merges dictionary keys
# in the order listed in the expression, overwriting
# duplicates from left to right.
#
# See:

">https://t.dripemail2.com/c/eyJhY2NvdW50X2lkIjoiNjIxNDUwMCIsImRlbGl2ZXJ5X2lkIjoiYnV1azgzbDhtN3oydmo4YXhnbXYiLCJ1cmwiOiJodHRwczovL3d3dy55b3V0dWJlLmNvbS93YXRjaD92PUR1ZXh3MDhLYUM4XHUwMDI2X19zPWxjdGdqaGZ2cmNveWl1NHk0NW5pIn0
```

#### TUPLES

Utiliser un tuple nommé peut s'avérer plus rapide que de définir une classe :

```py
# Namedtuples
# Using namedtuple is way shorter than
# defining a class manually:
>>> from collections import namedtuple
>>> Car = namedtuple('Car', 'color mileage')

# Our new "Car" class works as expected:
>>> my_car = Car('red', 3812.4)
>>> my_car.color
'red'
>>> my_car.mileage
3812.4

# We get a nice string repr for free:
>>> my_car
Car(color='red' , mileage=3812.4)

# Like tuples, namedtuples are immutable:
>>> my_car.color = 'blue'
AttributeError: "can't set attribute"
```

#### TEMPS

Mesurer la durée d'exécution d'un bout de code avec timeit :

```py
# The "timeit" module lets you measure the execution
# time of small bits of Python code

>>> import timeit
>>> timeit.timeit('"-".join(str(n) for n in range(100))',
            number=10000)

0.3412662749997253

>>> timeit.timeit('"-".join([str(n) for n in range(100)])',
            number=10000)

0.2996307989997149

>>> timeit.timeit('"-".join(map(str, range(100)))',
            number=10000)

0.24581470699922647
```

#### FONCTIONS

Function argument unpacking:

```py
# Function argument unpacking

def myfunc(x, y, z):
print(x, y, z)

tuple_vec = (1, 0, 1)
dict_vec = {'x': 1, 'y': 0, 'z': 1}

>>> myfunc(*tuple_vec)
1, 0, 1

>>> myfunc(**dict_vec)
1, 0, 1
```

Functions are first-class citizens in Python:

```py
# Functions are first-class citizens in Python:

# They can be passed as arguments to other functions,
# returned as values from other functions, and
# assigned to variables and stored in data structures.

>>> def myfunc(a, b):
...     return a + b
...
>>> funcs = [myfunc]
>>> funcs[0]

>>> funcs[0](2, 3)
5
```

On peut utiliser une fonction pour émuler un switch/case:

```py
# Because Python has first-class functions they can
# be used to emulate switch/case statements

def dispatch_if(operator, x, y):
if operator == 'add':
return x + y
elif operator == 'sub':
return x - y
elif operator == 'mul':
return x * y
elif operator == 'div':
return x / y
else:
return None

def dispatch_dict(operator, x, y):
return {
'add': lambda: x + y,
'sub': lambda: x - y,
'mul': lambda: x * y,
'div': lambda: x / y,
}.get(operator, lambda: None)()

>>> dispatch_if('mul', 2, 8)
16

>>> dispatch_dict('mul', 2, 8)
16

>>> dispatch_if('unknown', 2, 8)
None

>>> dispatch_dict('unknown', 2, 8)
None
```

#### SERVEUR WEB

On peut créer un serveur web basique avec la librairie standard de Python:

```py
# Python has a HTTP server built into the
# standard library. This is super handy for
# previewing websites.

# Python 3.x
$ python3 -m http.server

# Python 2.x
$ python -m SimpleHTTPServer 8000

# (This will serve the current directory at
#  http://localhost:8000)
```

#### PIP

Installation :

```bash
aptitude install python3-pip
```

#### ENVIRONNEMENTS VIRTUELS

Installation de virtualenv (en su -) (Debian 10):

```bash
pip3 install virtualenv
```

(Debian 12):

```bash
aptitude install python3-virtualenv
```

Création du projet my_project dans un environnement virtuel :

```bash
virtualenv -p python3.7 my_project
cd my_project
```

Activer l’environnement virtuel :

```bash
source bin/activate
```

Installer un module (ici Django) dans l'environnement virtuel :

```bash
pip install Django
```

Installer des modules à partir d'un fichier :

```bash
pip install -r requirements.txt
```

Tester la version de Django :

```py
>>> import django;
>>> print(django.get_version())
```

#### DÉMARRER UN PROJECT DJANGO

Création de l’application Django :

```bash
django-admin startproject my_project
cd my_project
```

Démarrer l'application sur 'http://localhost:8003' :

```bash
./manage.py runserver 8003
```

#### TESTS DJANGO

Liste des test à faire :

```
test_models.py:
- full name
- absolute_url
- labels, help_texts etc.
- max length
test_views.py:
- resolve(<url>) uses the right view
- login_required
- redirections
- user’s access to that page/item
- 200 with direct and reverse urls
- 404
- contains links to…
- initial values of forms
- redirect after form.is_valid()
- uses correct template
- pagination
- items ordered by…
test_forms.py:
- form.is_valid()
- contains widget
- labels, help_texts etc.
```

Les différents niveaux de tests : module, type (fichier), classe, fonction :

```bash
./manage.py test [module.tests[.test_models[.TestClass[.unit_test]]]]
```

Avec coverage et une sortie html :

```bash
coverage run manage.py test; coverage html
```

#### DJANGO TEMPUS-DOMINUS

Installer tempus-dominus :

```bash
pip install django-tempus-dominus
```

Ensuite dans settings.py :

```
1. Installed apps: ajouter ‘tempus_dominus’.
2. Ajouter tout en bas une option pour dire à Django de ne pas aller chercher les scripts sur le Cloud : TEMPUS_DOMINUS_INCLUDE_ASSETS = False
```

Ensuite installer lesdits scripts dans le dossier static, en allant les chercher sur le Cloud :

```
https://cdnjs.cloudflare.com/ajax/libs/tempusdominus-bootstrap-4/5.1.2/js/tempusdominus-bootstrap-4.min.js
https://cdnjs.cloudflare.com/ajax/libs/tempusdominus-bootstrap-4/5.1.2/css/tempusdominus-bootstrap-4.min.css
https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.23.0/moment.min.js
```

Enfin bien sûr indiquer le chemin de ces scripts dans le main template.html.

#### DJANGO SUR UN SERVEUR

Ajouter un virtual host dans /etc/apache2/sites-available/000-default.conf :

```apache
NameVirtualHost *:8000
<VirtualHost *:8000>
ServerName python.asj.com
ServerAdmin editeur@traditions-monastiques.com
DocumentRoot /home/frromain/Sites/infirmerie/infirmerie
WSGIDaemonProcess infirmerie.asj.com processes=2 threads=15 display-name=%{GROUP} python-home=/home/frromain/Sites/infirmerie python-path=/home_frromain/Sites/infirmerie/infirmerie
WSGIProcessGroup infirmerie.asj.com
WSGIApplicationGroup %{GLOBAL}
WSGIScriptAlias / /home/frromain/Sites/infirmerie/infirmerie/infirmerie/wsgi.py
<Directory /home/frromain/Sites/infirmerie/infirmerie/infirmerie>
<Files wsgi.py>
    Require all granted
</Files>
</Directory>

Alias /static/ /home/frromain/Sites/infirmerie/infirmerie/static_files/
<Directory /home/frromain/Sites/infirmerie/infirmerie/static_files>
Require all granted
</Directory>

ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Commentaires :
Le dossier static est celui dans lequel collectstatic va placer tous les fichiers statiques.
S'il y a plusieurs sites distingués uniquement par le port, ajouter un "listen" par port en tête des virtual hosts :

```apache
listen 8000
listen 8001
listen 8002
```

Installer le module wsgi pour Apache :

```bash
aptitude install libapache2-mod-wsgi-py3
```

Si besoin de MySQL (Debian 10):

```bash
aptitude install libssl-dev python3.7-dev libmysqlclient-dev
```

(Debian 12):

```bash
aptitude install python3-dev default-libmysqlclient-dev build-essential pkg-config
```

Modifier settings.py :

```py
DEBUG = False
…
ALLOWED_HOSTS = ['*']
…
TEMPLATES = [
…
'DIRS': ['/home/frromain/Sites/infirmerie/infirmerie/templates'],
…
]
…
STATIC_URL = '/static/'
STATIC_ROOT = '/home/frromain/Sites/infirmerie/infirmerie/static_files/'
```

Modifier wsgi.py :

```py
import os
import sys
paths = [
'/home/frromain/Sites/infirmerie/infirmerie',
'/home/frromain/Sites/infirmerie/lib/python3.7/site-packages',
]
for path in paths:
if path not in sys.path:
sys.path.append(path)

from django.core.wsgi import get_wsgi_application
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'infirmerie.settings')
application = get_wsgi_application()
```
