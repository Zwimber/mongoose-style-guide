### Quicly clean all your files with prettier

`npm install prettier & npx prettier --write ./\*.js`

# MongoDB

### List all collections and their sizes

```js
var collectionNames = db.getCollectionNames();
var stats = [];

collectionNames.forEach((n) => stats.push(db[n].stats()));
var statsSorted = stats.sort((a, b) => a.size < b.size);

for (var c in statsSorted) {
  const gbRaw = stats[c]["size"] / 1024 / 1024 / 1024;
  const gb = gbRaw.toFixed(2);
  print(stats[c]["ns"] + " - " + stats[c]["count"] + " items");
  print(gb + " Gb");
  print("");
}
```
