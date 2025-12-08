# Økt 3 – Tastestyring, kollisjon og vei mot Flappy Bird
**Underviser-guide – JavaScript Canvas / demo8**  

Utgangspunkt: eksisterende `demo8.html` med:
- canvas
- bakgrunn som scroller
- figur med gravitasjon (fall nedover)
- game loop (f.eks. `drawRectangles` + `requestAnimationFrame`)

Målet for økten:
- styre en firkant/figur med tastatur (først posisjon, så fart)
- ha to bevegelige firkanter på skjermen
- implementere enkel kollisjonsdeteksjon mellom firkanter (AABB)
- bruke kollisjon til *game over*
- komme så nær en første versjon av Flappy Bird som mulig

Ingen eksplisitte elevoppgaver – dette er en ren gjennomføringsplan for økten.

---

## Oversikt – foreslått flyt

1. Kort recap av demo8 og mål for økta
2. Tastestyring for én firkant (posisjon → fart)
3. To firkanter med hver sin fart
4. Kollisjon mellom firkanter (AABB – axis aligned bounding boxes)
5. Flappy Bird-retning:
   - space = hopp
   - bakken = game over
   - én stolpe + kollisjon
   - ev. en stolpe til + enkel refaktor
   - hvis tid: score og/eller restart

---

## 1. Recap og rød tråd (5–10 min)

- Vis `demo8.html` og forklar kort hva som allerede er på plass:
  - canvas
  - bakgrunn
  - figur som faller pga. gravitasjon
  - game loop-funksjon (f.eks. `drawRectangles`)
- Si eksplisitt at **i dag skal vi begynne å gjøre dette til et spill**:
  - vi skal styre figuren med tast
  - vi skal ha andre ting som beveger seg
  - vi skal vite når ting treffer hverandre (kollisjon)

Kort rød tråd:
> Først bevegelse (tast & fart), så flere ting som beveger seg, så kollisjon, så Flappy.

---

## 2. Tastestyring – først direkte posisjon, så fart (ca. 15–20 min)

### 2.1 Enkel tastestyring – flytte posisjon direkte

Lag (eller vis) en enkel variant der en firkant kan flyttes rundt med piltaster.

Eksempel (konseptuelt – tilpass til din kode):

```js
let playerX = 50;
let playerY = 50;
const playerWidth = 40;
const playerHeight = 40;

let isLeftPressed = false;
let isRightPressed = false;
let isUpPressed = false;
let isDownPressed = false;

document.addEventListener('keydown', (event) => {
    if (event.code === 'ArrowLeft') isLeftPressed = true;
    if (event.code === 'ArrowRight') isRightPressed = true;
    if (event.code === 'ArrowUp') isUpPressed = true;
    if (event.code === 'ArrowDown') isDownPressed = true;
});

document.addEventListener('keyup', (event) => {
    if (event.code === 'ArrowLeft') isLeftPressed = false;
    if (event.code === 'ArrowRight') isRightPressed = false;
    if (event.code === 'ArrowUp') isUpPressed = false;
    if (event.code === 'ArrowDown') isDownPressed = false;
});
```

I game loop-en (f.eks. i `drawRectangles`), før tegningen:

```js
const speed = 4;

if (isLeftPressed) playerX -= speed;
if (isRightPressed) playerX += speed;
if (isUpPressed) playerY -= speed;
if (isDownPressed) playerY += speed;
```

Og ved tegning:

```js
ctx.fillStyle = 'red';
ctx.fillRect(playerX, playerY, playerWidth, playerHeight);
```

### 2.2 Overgang til å styre fart

Neste steg: ikke endre posisjon direkte, men **fart**.

```js
let playerVX = 0;
let playerVY = 0;

const accel = 0.5;

if (isLeftPressed) playerVX -= accel;
if (isRightPressed) playerVX += accel;

// Friksjon / brems (enkel variant)
playerVX *= 0.9;

// Oppdater posisjon
playerX += playerVX;
```

Her kan du velge hvor avansert du vil gjøre det (f.eks. bare styre X-fart).

Viktig forbindelse til demo8:
- Demo8 har allerede gravitasjon (VY påvirkes nedover)
- Nå viser du at **input = påvirkning av fart**, ikke “teleportering” av posisjon

---

## 3. To firkanter med hver sin fart (ca. 10–15 min)

Introduksjon:
- Legg til en **annen firkant** som bare beveger seg “av seg selv”

```js
let enemyX = 300;
let enemyY = 100;
const enemyWidth = 40;
const enemyHeight = 40;
let enemyVX = -2; // beveger seg mot venstre
```

I game loop-en:

```js
enemyX += enemyVX;

ctx.fillStyle = 'blue';
ctx.fillRect(enemyX, enemyY, enemyWidth, enemyHeight);
```

Eventuelt:
- la den sprette tilbake når den treffer kantene
- eller respawne på høyre side når den går ut på venstre

Poeng:
- nå har du **minst to ting som beveger seg samtidig**
- scenen er satt for kollisjon

---

## 4. Kollisjon mellom firkanter (AABB) (ca. 15–20 min)

Målet her:
- ha én funksjon som kan svare på “treffer disse to hverandre?”
- bruke den til å trigge “game over” eller stoppe animasjon

Vi tenker i termer av “ikke-kollisjon”:

To rektangler **kolliderer ikke** hvis:
- den ene er helt til venstre for den andre
- den ene er helt til høyre for den andre
- den ene er helt over den andre
- den ene er helt under den andre

Ellers er det kollisjon.

Lag en funksjon:

```js
function rectanglesCollide(r1, r2) {
    const r1Right = r1.x + r1.width;
    const r1Bottom = r1.y + r1.height;
    const r2Right = r2.x + r2.width;
    const r2Bottom = r2.y + r2.height;

    const noCollision =
        r1Right < r2.x ||
        r1.x > r2Right ||
        r1Bottom < r2.y ||
        r1.y > r2Bottom;

    return !noCollision;
}
```

Bruk den i game loop-en:

```js
const playerRect = { x: playerX, y: playerY, width: playerWidth, height: playerHeight };
const enemyRect = { x: enemyX, y: enemyY, width: enemyWidth, height: enemyHeight };

if (rectanglesCollide(playerRect, enemyRect)) {
    // for eksempel stoppe animasjonen:
    gameOver = true;
}
```

Hvis du har en `gameOver`-variabel i toppen:

```js
let gameOver = false;
```

Og i game loop-en:

```js
if (gameOver) {
    // Tegn en tekst og returner uten å oppdatere videre
    ctx.fillStyle = 'black';
    ctx.font = '30px Arial';
    ctx.fillText('Game Over', 100, 100);
    return;
}
```

Dette er første gang de ser et “ordentlig” game over.

**Kort kommentar om logiske operatorer:**  
Over er et naturlig sted å peke på at `||` betyr “eller” og `&&` kunne brukt i en alternativ formulering, uten å gjøre det til eget tema.

---

## 5. Vei mot Flappy Bird (resten av tiden)

Nå går du fra “sandbox” til noe som ligner Flappy Bird, gjerne ved å bygge videre på demo8.

### 5.1 Space = hopp, bakken = game over

Bruk demo8 sin figur med gravitasjon (f.eks. `martinY`, `martinVY` osv.).

Legg til input:

```js
let isSpacePressed = false;

document.addEventListener('keydown', (event) => {
    if (event.code === 'Space') {
        isSpacePressed = true;
    }
});

document.addEventListener('keyup', (event) => {
    if (event.code === 'Space') {
        isSpacePressed = false;
    }
});
```

I game loop-en:

```js
// Gravitasjon
martinVY += gravity;

// Hopp
if (isSpacePressed) {
    martinVY = -jumpStrength; // f.eks. -8 eller -10
}

martinY += martinVY;
```

For bakken (forutsatt at du har en “groundY” eller tilsvarende):

```js
if (martinY + martinHeight >= groundY) {
    gameOver = true;
}
```

Og fjern sprett-logikk hvis du hadde det tidligere.

### 5.2 Én stolpe + kollisjon

Lag en stolpe (rektangel) som beveger seg mot figuren:

```js
let pipeX = canvasWidth;
let pipeWidth = 60;
let pipeGapY = 150;      // hvor åpningen er
let pipeGapHeight = 120; // størrelsen på åpningen
let pipeSpeed = -3;
```

I game loop-en:

```js
pipeX += pipeSpeed;

ctx.fillStyle = 'green';
// Øvre stolpe
ctx.fillRect(pipeX, 0, pipeWidth, pipeGapY);
// Nedre stolpe
ctx.fillRect(pipeX, pipeGapY + pipeGapHeight, pipeWidth, canvasHeight - (pipeGapY + pipeGapHeight));
```

Bruk kollisjonsfunksjonen fra tidligere ved å lage passende rektangler for øvre og nedre stolpe og sjekke dem mot figuren.

### 5.3 En stolpe til + enkel refaktor (hvis tid)

Hvis tiden tillater det:
- vis hvordan du kan representere stolpene som et array av objekter
- løkke over arrayet og:
  - oppdatere posisjon
  - tegne
  - sjekke kollisjon

Eksempel struktur:

```js
const pipes = [
    { x: canvasWidth, width: 60, gapY: 150, gapHeight: 120 },
    { x: canvasWidth + 300, width: 60, gapY: 180, gapHeight: 120 }
];
```

I game loop-en, pseudo:

```js
for (const pipe of pipes) {
    pipe.x += pipeSpeed;
    // tegn øvre og nedre del
    // sjekk kollisjon
}
```

Score og restart kan tas dersom det er tid, men er ikke kritiske for hovedmålet med økten.

---

## Oppsummert fokus for økten

- Tastatur → kontroll over fart og bevegelse
- Flere bevegelige objekter på skjermen samtidig
- Kollisjon mellom firkanter (AABB) som første robuste spillmekanikk
- Første versjon av Flappy-lignende opplevelse:
  - gravitasjon
  - hopp med space
  - hindring(er)
  - game over ved kollisjon / bakken
