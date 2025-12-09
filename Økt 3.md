# Økt 3 – Tastestyring, kollisjon og «Flappy Martin»
**JavaScript Canvas – stegvis progresjon**

I denne økten jobber vi med tre korte og tydelige deler:
1. En helt enkel firkant for repetisjon av game loop og tastestyring  
1. demo4.html for å introdusere kollisjon og game over  
1. Tilfeldige tall og farger
1. demo8.html og veien videre mot «Flappy Martin»  

Målet er å lære noen få, viktige mekanikker – og bruke dem flere ganger.

---

## Del 1 – Repetisjon: én enkel firkant + tastestyring

Vi starter med et helt enkelt oppsett:
- én firkant
- posisjon `x` og `y`
- en game loop med `requestAnimationFrame`

Formålet er å repetere grunnmønsteret før vi går videre.

### 1.1 Game loop og tegning

Firkanten tegnes basert på nåværende `x`- og `y`-posisjon.
Game loop-en oppdaterer posisjon og tegner på nytt for hver frame.

Rød tråd:
> Game loop = oppdater state → tegn basert på state → neste frame

---

### 1.2 Tastestyring – styre posisjon direkte

Vi legger først inn enkel tastestyring der tastene endrer posisjon direkte.
Dette er bevisst «naivt», og brukes bare for å komme raskt i gang.

#### Variabler

```js
let leftPressed = false;
let rightPressed = false;
let upPressed = false;
let downPressed = false;
```

#### Tastelyttere

```js
function handleKeyDown(event) {
    if (event.code === 'ArrowLeft') leftPressed = true;
    if (event.code === 'ArrowRight') rightPressed = true;
    if (event.code === 'ArrowUp') upPressed = true;
    if (event.code === 'ArrowDown') downPressed = true;
}

function handleKeyUp(event) {
    if (event.code === 'ArrowLeft') leftPressed = false;
    if (event.code === 'ArrowRight') rightPressed = false;
    if (event.code === 'ArrowUp') upPressed = false;
    if (event.code === 'ArrowDown') downPressed = false;
}

document.addEventListener('keydown', handleKeyDown);
document.addEventListener('keyup', handleKeyUp);
```

I game loop-en:
- hvis `leftPressed` → reduser `x`
- hvis `rightPressed` → øk `x`
- tilsvarende for `y`

Dette fungerer, men er ikke slik spill vanligvis bygges.

---

### 1.3 Tastestyring – styre fart i stedet for posisjon

Neste forbedring:
- tastene påvirker **fart**
- farten påvirker posisjon

Vi introduserer:
- `vx` og `vy`
- posisjon oppdateres basert på fart

Rød tråd:
> Input → endrer fart → fart endrer posisjon

Dette er samme mønster som vi senere bruker i demo8 med gravitasjon.


---

## Del 2 – demo4.html: Kollisjon og game over

Nå skifter vi eksempel helt bevisst.

Vi åpner `demo4.html`, som allerede inneholder:
- en firkant som beveger seg automatisk
- fart i x- og y-retning
- sprett i kantene

### 2.1 Legge til en fast firkant

Vi legger til:
- én firkant med fast posisjon midt på skjermen
- denne firkanten beveger seg ikke

Nå har vi:
- én firkant i bevegelse
- én stillestående firkant

Dette er et perfekt utgangspunkt for kollisjon.

---

### 2.2 Kollisjon mellom firkanter (AABB)

#### Hva er AABB?

AABB står for **Axis-Aligned Bounding Box**.

- *Axis-aligned*: firkantene er ikke rotert
- *Bounding box*: vi bruker en enkel firkant som representerer objektet

Dette er den vanligste og enkleste kollisjonsmetoden i 2D-spill.

---

#### Prinsipp

To firkanter kolliderer **ikke** hvis:
- den ene er helt til venstre for den andre
- eller helt til høyre
- eller helt over
- eller helt under

Hvis ingen av disse stemmer, har vi kollisjon.

---

#### Kollisjonsfunksjon

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

Vi bruker denne funksjonen i game loop-en:
- ved kollisjon stopper vi animasjonen
- dette er vårt første *game over*

Rød tråd:
> Kollisjon handler ikke om grafikk – bare om tall.

## Del 3 Tilfeldige tall og farger

`Math.random()` osv. 

---

## Del 4 – demo8.html: Mot «Flappy Martin»

Til slutt går vi tilbake til `demo8.html`.

Vi ser kort på hva som allerede finnes:
- gravitasjon
- bakgrunn som beveger seg
- en figur som faller

Nå kan vi bruke verktøyene vi nettopp har lært.

---

### 3.1 Tastestyring: space = hopp

Vi bruker samme boolean-mønster som tidligere.

```js
let spacePressed = false;

function handleKeyDown(event) {
    if (event.code === 'Space') spacePressed = true;
}

function handleKeyUp(event) {
    if (event.code === 'Space') spacePressed = false;
}
```

I game loop-en:
- hvis `spacePressed` → gi figuren et oppover-kick i y-fart
- gravitasjonen trekker figuren ned igjen

---

### 3.2 Bakken er ikke sprett – bakken er tap

I demo8 var bakken tidligere noe figuren spratt på.

Nå endrer vi betydningen:
- bakken er en kollisjon
- kollisjon med bakken gir *game over*

Dette er samme kollisjonsidé som i demo4 – bare med en annen reaksjon.

---

### 3.3 Stolper og kollisjon

Vi legger til stolper:
- stolper er firkanter
- de beveger seg mot spilleren

Ved kollisjon mellom:
- figur og stolpe
- figur og bakken

→ *game over*

Samme kollisjonsfunksjon brukes hele veien.

## Bonus – Score og restart (hvis vi får tid)

Dette er ikke nødvendig for å forstå Flappy-mekanikkene, men gir et mer komplett spill.
Hvis vi ikke rekker alt, er dette et naturlig sted å fortsette neste gang.

---

## Score

### Hva mener vi med score?

I Flappy Bird betyr score vanligvis:
- antall stolper vi har passert uten å krasje

Score er bare:
- en variabel
- som øker under bestemte betingelser
- og tegnes på skjermen

---

### Starte med score-variabel

```js
let score = 0;
```

---

### Øke score når en stolpe passeres

Når en stolpe beveger seg mot venstre:
- og går forbi spilleren
- uten at det ble kollisjon

kan vi øke score én gang.

En enkel strategi:
- hver stolpe har et flagg `hasPassed`
- når stolpens høyre kant går forbi spillerens x-posisjon:
  - øk score
  - sett `hasPassed = true`

Eksempel (konseptuelt):

```js
if (!pipe.hasPassed && pipe.x + pipe.width < playerX) {
    score++;
    pipe.hasPassed = true;
}
```

Poenget:
- samme stolpe skal bare gi poeng én gang

---

### Tegne score på skjermen

I game loop-en, etter at bakgrunnen er tegnet:

```js
ctx.fillStyle = 'black';
ctx.font = '24px Arial';
ctx.fillText('Score: ' + score, 20, 30);
```

---

## Restart

Når spillet er over:
- stopper animasjonen
- spilleren kan ikke gjøre noe mer

Neste steg er å kunne starte på nytt.

---

### Hva må resettes ved restart?

For å starte spillet på nytt må vi:
- sette spillerens posisjon tilbake
- nullstille fart (vx / vy)
- nullstille score
- flytte stolper tilbake til start
- sette `gameOver = false`

Dette er ofte mer jobb enn man først tror.

---

### Samle restart-logikk i én funksjon

En ryddig løsning er å ha en egen funksjon:

```js
function restartGame() {
    score = 0;
    gameOver = false;

    playerX = startX;
    playerY = startY;
    playerVX = 0;
    playerVY = 0;

    resetPipes();
}
```

Poenget:
Restart er ikke magi – vi bare setter state tilbake til startverdier.

---

### Starte restart med tastetrykk

For eksempel:
- trykk `Enter` for å starte på nytt

```js
if (gameOver && enterPressed) {
    restartGame();
}
```

Her gjenbruker vi:
- samme tastestyringsmønster som tidligere
- samme game loop

---

## Viktig poeng

Score og restart lærer oss at:
- et spill egentlig bare er state
- game over er bare en bestemt tilstand
- restart er bare å sette state tilbake

Dette er samme tenkning vi bruker i større applikasjoner også.
