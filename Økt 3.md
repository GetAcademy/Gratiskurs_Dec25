# Økt 3 – Tastestyring, kollisjon og «Flappy Martin»
**JavaScript Canvas – stegvis progresjon**

I denne økten jobber vi med tre korte og tydelige deler:
1. En helt enkel firkant for repetisjon av game loop og tastestyring  
2. demo4.html for å introdusere kollisjon og game over  
3. demo8.html og veien videre mot «Flappy Martin»  

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

---

## Del 3 – demo8.html: Mot «Flappy Martin»

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

---

## Oppsummering av økten

I denne økten har vi:
- repetert game loop og bevegelse
- lært tastestyring via boolean per tast
- introdusert kollisjon med AABB
- brukt kollisjon til å avgjøre *game over*
- bygget videre mot et faktisk spill

Alt bygger på samme enkle ideer – brukt i stadig rikere eksempler.
