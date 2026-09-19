# Map Generator (probabletrain) — installation locale du 16.09.2026

Clone de https://github.com/probabletrain/mapgenerator (LGPL-3.0), testé sur cette machine : le build préconstruit tourne, la compilation du code source aussi. Léger : la génération d'une ville complète avec bâtiments prend ~3 s de CPU et quelques centaines de Mo de RAM, pas besoin du nouveau PC.

## Lancer (version préconstruite, sans compiler)

```powershell
cd C:\www\lausanne-map-generator
python -m http.server 8090 -d dist
```

Ouvrir http://localhost:8090 dans Chrome. Clic « generate », puis Style → colourScheme (GoogleNoZoom pour les bâtiments 3D), Download → PNG / SVG / STL / Heightmap.

## Modifier le code et recompiler

```powershell
cd C:\www\lausanne-map-generator
npx gulp          # compile src/ vers dist/bundle.js puis surveille les fichiers .ts (Ctrl+C pour arrêter)
```

Les dépendances sont déjà installées (`npm install` fait, 840 paquets, toolchain gulp 4 + browserify + TypeScript 3.8, fonctionne avec Node 24).

## Ce que l'outil fait et ne fait pas

- Génère des villes FICTIVES de style américain à partir d'un champ de tenseurs (grilles + radiales) : eau, routes principales / majeures / mineures, parcs, parcelles, bâtiments pseudo-3D.
- N'importe AUCUNE donnée réelle : ni OpenStreetMap, ni swisstopo (vérifié dans `src/`). Il ne peut donc pas « générer Lausanne ».
- Exports : PNG, SVG (polygones des îlots et bâtiments, exploitable comme données de jeu), STL (zip : bâtiments, îlots, routes, rivière, mer), heightmap.

## Usage envisagé pour le RPG Lausanne

1. Base réelle : swisstopo (swissBUILDINGS3D + swissALTI3D) via BlenderGIS, voir la note PARA « Modéliser Lausanne fidèlement ».
2. Extensions fictives de 2059 (quartiers verticaux, zones corporatistes) : générées ici avec un champ de tenseurs orienté comme les axes réels, exportées en SVG/STL, posées sur la base réelle dans Blender.
3. Fork possible (1 à 2 jours) : ajouter un « champ polyligne » dans `src/ts/impl/basis_field.ts` pour semer le champ de tenseurs avec les routes réelles de swissTLM3D, et ne générer que les rues mineures et les bâtiments autour. Le code fait 950 lignes de TypeScript propre, découpé en `impl/` (algorithme) et `ui/` (rendu).

## Alternatives à comparer

- Watabou, Medieval Fantasy City Generator et City Viewer (https://watabou.itch.io) : villes fictives, export JSON/SVG, très populaire chez les rôlistes.
- blosm / BlenderGIS pour les données réelles (voir note PARA).
