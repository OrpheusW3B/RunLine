# RUNLINE

Jeu de parkour à la première personne inspiré de l'esthétique de *Mirror's Edge*.

## Lancer le jeu

**Ne double-clique pas sur `index.html`** : Chrome bloque les modules ES avec une adresse `file://` pour des raisons de sécurité.

### Application de bureau (Electron) — recommandé

```bash
npm install
npm start
```

Le jeu s'ouvre dans sa propre fenêtre, avec un menu complet : **Jouer**, **Réglages**, **Crédits** et **Quitter**.

### Construire l'installeur Windows (.exe)

```bash
npm run dist
```

Produit dans `dist/` :

- `RUNLINE-Setup-1.0.0.exe` — installeur complet (raccourcis Bureau + Menu Démarrer, désinstalleur, dossier d'installation au choix) ;
- `RUNLINE-Portable-1.0.0.exe` — version portable, sans installation.

Le jeu est **100 % hors-ligne** : le moteur Three.js est embarqué dans l'application (aucun CDN requis).

### Ajouter à Steam (comme un vrai jeu)

1. Steam → **Ajouter un jeu** → **Ajouter un jeu non Steam** → **Parcourir** ;
2. Choisis `dist/RUNLINE.exe` (ou le `RUNLINE.exe` installé) ;
3. Tu peux renommer l'entrée « RUNLINE » et jouer avec l'overlay Steam (F12, captures, manette…).

## Mises à jour automatiques

Le jeu vérifie les mises à jour au démarrage et dans le menu **Réglages** : téléchargement en arrière-plan puis installation au redémarrage.

#### Windows

Deux options, selon ta configuration :

1. **`LANCER-RUNLINE.bat`** — double-clique dessus. Il détecte Python, démarre le serveur et ouvre le jeu. Si Python manque, il affiche les instructions.
2. **`LANCER-RUNLINE.ps1`** — clic droit → « Run with PowerShell ». Plus fiable si `cmd` est bloqué ou si Python n'est pas dans le `PATH` du terminal.

## Structure

- `index.html` : point d'entrée et structure des overlays/HUD
- `styles/main.css` : styles de l'interface
- `scripts/main.js` : boucle de rendu et orchestration
- `electron-main.cjs` / `electron-preload.cjs` : application de bureau (Electron)
- `package.json` : manifeste Electron (`npm install` puis `npm start`)
- `scripts/scene.js` : scène Three.js, caméra, lumières et matériaux
- `scripts/level.js` : trois géométries de parcours et décor généré
- `scripts/audio.js` : mixage Web Audio, samples distants et fallback procédural
- `scripts/player.js` : physique, déplacements, mains animées et caméra
- `scripts/map-wind.js` : fanions et rubans 3D animés dans le décor
- `scripts/hud.js` : timer, sélection de niveau, progression et messages d'action

## Niveaux et audio

Le menu propose **six routes** faites main : `Skyline Sprint` (initiation), `Switchback` (élan), `Afterglow` (expert), `Vertigo` (descente en cascade), `Redline` (vitesse pure, slides sous poutres) et `Ascent` (ascension en mantles et wall-jumps). Chaque niveau a son tracé, ses panneaux indicateurs et sa ligne d'arrivée — le chrono s'arrête à la porte.

### Grimpe partout

- **SPACE** contre n'importe quelle corniche (depuis le sol comme en plein saut) → tu grimpes ;
- **wall-run** sur n'importe quel mur de plus de 3 m de haut, plus seulement les murs rouges ;
- les mains **cubiques** s'ancrent réellement sur les obstacles pendant vault, mantle et wall-run.

Sur mobile, les boutons tactiles et le glissement sur l'écran remplacent le clavier et la souris.

Le vent est uniquement visible **sur la carte** via des fanions et rubans 3D ; aucun effet de traînée ou de distorsion ne recouvre plus l'écran à grande vitesse. Le déplacement aérien est plus précis et le saut est variable : appui court pour un petit saut, maintien de SPACE pour atteindre la hauteur maximale.

Les pas de course, impacts, roulades et saisies utilisent des sons **CC0 de Kenney** (kenney.nl, paquet *Impact Sounds*), servis depuis GitHub avec les en-têtes CORS. Ils sont chargés après le premier clic ; si le réseau ou le navigateur les bloque, le jeu bascule automatiquement sur ses sons Web Audio de secours.
