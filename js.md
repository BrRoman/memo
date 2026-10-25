# Javascript / Node / Express / MongoDB / Electron

##### Les chevrons (>>>) indiquent que la commande doit être exécutée dans un shell Mongo.

#### SOUS-CHAÎNES :

Avec substring(start, end) :

```js
const anyString = "Mozilla";
console.log(anyString.substring(0, 1)); // "M"
console.log(anyString.substring(1, 0)); // "M"
console.log(anyString.substring(0, 6)); // "Mozill"
console.log(anyString.substring(4)); // "lla"
console.log(anyString.substring(4, 7)); // "lla"
console.log(anyString.substring(7, 4)); // "lla"
console.log(anyString.substring(0, 7)); // "Mozilla"
console.log(anyString.substring(0, 10)); // "Mozilla"
```

substring() en combinaison avec length :

```js
const text = "Mozilla";
// Takes 4 last characters of string
console.log(text.substring(text.length - 4)); // prints "illa"
// Takes 5 last characters of string
console.log(text.substring(text.length - 5)); // prints "zilla"
```

Avec slice(start, end) :

```js
const str1 = "The morning is upon us."; // The length of str1 is 23.
const str2 = str1.slice(1, 8);
const str3 = str1.slice(4, -2);
const str4 = str1.slice(12);
const str5 = str1.slice(30);
console.log(str2); // he morn
console.log(str3); // morning is upon u
console.log(str4); // is upon us.
console.log(str5); // ""
```

slice() avec index négatifs :

```js
const str = "The morning is upon us.";
str.slice(-3); // 'us.'
str.slice(-3, -1); // 'us'
str.slice(0, -1); // 'The morning is upon us'
str.slice(4, -1); // 'morning is upon us'
```

**Differences between substring() and slice()**

The substring() and slice() methods are almost identical, but there are a couple of subtle differences between the two, especially in the way negative arguments are dealt with.
The substring() method swaps its two arguments if indexStart is greater than indexEnd, meaning that a string is still returned. The slice() method returns an empty string if this is the case.

```js
const text = "Mozilla";
console.log(text.substring(5, 2)); // "zil"
console.log(text.slice(5, 2)); // ""
```

If either or both of the arguments are negative or NaN, the substring() method treats them as if they were 0.

```js
console.log(text.substring(-5, 2)); // "Mo"
console.log(text.substring(-5, -2)); // ""
```

slice() also treats NaN arguments as 0, but when it is given negative values it counts backwards from the end of the string to find the indexes.

```js
console.log(text.slice(-5, 2)); // ""
console.log(text.slice(-5, -2)); // "zil"
```

#### NODE :

Récupérer l'installateur, puis installer Node et npm (remplacer NN par le numéro de la dernière version) :

```bash
curl --proxy http://192.168.1.250:8080 -sL https://deb.nodesource.com/setup_NN.x | bash -
apt-get install -y nodejs
```

Configuration de npm (dans le dossier home) :

```bash
npm config set proxy http://192.168.1.250:8080
npm config set https-proxy http://192.168.1.250:8080
npm config set strict-ssl false
```

Liste des config de npm :

```bash
npm config list
```

#### YARN (alternative à NPM) :

Configuration du repository :

```bash
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
```

Installation :

```bash
apt update && sudo apt install yarn
```

Vérification :

```bash
yarn --version
```

#### EXPRESS :

Installation d'express-generator, puis d'un skeleton Express (avec Pug comme moteur de template) :

```bash
npm install express-generator -g
express my_project --view=pug
cd my_project
npm install
DEBUG=my_project:* npm start
```

Installation de nodemon (to enable server restart on every change) :

```bash
npm install --save-dev nodemon
```

Puis dans package.json :

```json
"scripts": {
"start": "node ./bin/www",
"devstart": "nodemon ./bin/www",
"serverstart": "DEBUG=my_project:* npm run devstart"
},
```

En cas de problème du type "[nodemon] Internal watch failed: ENOSPC: System limit for number of file watchers reached…", augmenter (en tant que su) le nombre de max_user_watches :

```bash
sysctl -w fs.inotify.max_user_watches=165536
```

#### INSTALLATION DE MONGODB :

1. Récupérer la clé :

```bash
wget -qO - https://www.mongodb.org/static/pgp/server-5.0.asc | apt-key add -
```

2. Ajouter à la source list :

```bash
echo "deb http://repo.mongodb.org/apt/debian buster/mongodb-org/5.0 main" | tee /etc/apt/sources.list.d/mongodb-org-5.0.list
apt-get update
apt-get install -y mongodb-org
```

3. Démarrer :

```bash
systemctl start mongod
```

Enable on restart :

```bash
systemctl enable mongod
```

Vérifications :

```bash
systemctl status mongod
systemctl stop mongod
systemctl restart mongod
```

Démarrer un shell mongo :

```bash
mongo
```

#### MONGODB : COMMANDES USUELLES :

Login :

```bash
mongo -u <username> -p <password> --authenticationDatabase <dbname>
```

Show databases :

```bash
>>> show dbs
```

Select db :

```bash
>>> use databaseName
```

Liste des collections :

```bash
>>> show collections
```

Créer une collection :

```bash
>>> db.createCollection("collectionName")
```

Insert :

```bash
>>> db.<collectionName>.insert({field1: "value", field2: "value"})
>>> db.<collectionName>.insert([{field1: "value1"}, {field1: "value2"}])
```

Update :

```bash
>>> db.<collectionName>.save({"_id": new ObjectId("jhgsdjhgdsf"), field1: "value", field2: "value"})
```

Display :

```bash
>>> db.<collectionName>.find();
>>> db.<collectionName>.find({"_id": ObjectId("someid")});
>>> db.<collectionName>.find({"_id": ObjectId("someid")}, {field1: 1, field2: 1})
>>> db.<collectionName>.find({"_id": ObjectId("someid")}, {field1: 0})
```

Count :

```bash
>>> db.<collectionName>.count();
```

Tout supprimer :

```bash
>>> db.<collectionName>.remove({});
```

Commandes administrateur :

```bash
>>> db.<collectionName>.stats()
>>> db.<collectionName>.dataSize()
```

Toutes ces commandes peuvent être exécutées dans un shell bash grâce à l'option --eval de la commande mongo + nom de la base de données :

```bash
mongo polyglotte --eval 'db.verses.drop()'
```

Exporter une db vers un fichier :

```bash
mongoexport --db=polyglotte --collection=verses --out=polyglotte.json
```

Importer une db depuis un fichier :

```bash
mongoimport --db=polyglotte --collection=verses --file=polyglotte.json
```

#### NOSQL client (interface graphique pour Mongo) :

Installer meteor :

```bash
curl https://install.meteor.com/ | sh
```

Dans le répertoire (cloné) de nosql-client :

```bash
meteor --port 3113
```

#### ELECTRON :

Installation :

```bash
ELECTRON_GET_USE_PROXY=true GLOBAL_AGENT_HTTPS_PROXY=http://192.168.1.250:8080 npm install --save-dev electron
```

#### METEOR :

Installation (sudo) :

```bash
curl --proxy http://192.168.1.250:8080 https://install.meteor.com | sh
```

Meteor behind a proxy :

```bash
export HTTP_PROXY=http://192.168.1.250:8080
export HTTPS_PROXY=http://192.168.1.250:8080
meteor update
```

Créer une application :

```bash
meteor create --react my_app
cd my_app
meteor
```

Packages utiles :

```bash
meteor npm install less popper.js bootstrap
```

Pour résoudre le problème du max_user_watches, lancer cette commande en tant que su :

```bash
echo fs.inotify.max_user_watches = 524288 | tee -a /etc/sysctl.conf && sudo sysctl -p
vim /proc/sys/fs/inotify/max_user_watches
```

Pour connaître l'url de la mini database mongo :

```bash
meteor mongo -U
=> mongodb://127.0.0.1:3001/meteor
```

Pour se connecter à cette database via un shell mongo :

```bash
mongo "mongodb://127.0.0.1:3001/meteor"
```
