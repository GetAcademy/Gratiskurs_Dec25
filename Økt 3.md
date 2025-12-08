# Økt 3 – Tastestyring, kollisjon og vei mot Flappy Bird
**JavaScript Canvas – videre på demo8**

Utgangspunkt:
- `demo8.html` med canvas
- bakgrunn som scroller
- figur som påvirkes av gravitasjon
- game loop (`requestAnimationFrame`)

Målet for økten:
- styre figuren med tastatur
- forstå fart i x- og y-retning
- introdusere kollisjon mellom firkanter
- bruke kollisjon til *game over*
- bevege oss helt frem til en enkel Flappy Bird

---

## Oversikt over økten

1. Vi ser på demo8.html og hva som allerede er på plass  
2. Tastestyring med boolean per tast  
3. To firkanter som beveger seg samtidig  
4. Kollisjon mellom firkanter (AABB)  
5. Gradvis overgang til Flappy Bird  
   - space = hopp  
   - bakken = game over  
   - stolper + kollisjon  

---

## 1. demo8.html – utgangspunkt og rød tråd

Vi ser på `demo8.html` og hva som allerede er på plass:
- canvas og koordinatsystem
- game loop som kjører kontinuerlig
- en figur som faller nedover på grunn av gravitasjon

Rød tråd for økten:
> Først lærer vi å styre figuren → deretter lar vi flere ting bevege seg → til slutt avgjør vi når ting treffer hverandre.

---

## 2. Tastestyring med boolean per tast

Vi bruker én boolean per tast.
Dette gjør at figuren kan bevege seg **så lenge tasten holdes inne**.

### 2.1 Variabler

```js
let leftPressed = false;
let rightPressed = false;
let upPressed = false;
let downPressed = false;
```

### 2.2 Tastetrykk

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

### 2.3 Bruk i game loop (konsept)

I game loop-en bruker vi boolean-ene til å:
- enten flytte figuren direkte
- eller (helst) justere fart i x- og y-retning

Eksempel i tekst:
- hvis `leftPressed` → reduser fart i x-retning
- hvis `rightPressed` → øk fart i x-retning

Dette bygger naturlig videre på gravitasjonen som allerede påvirker farten i y-retning.

---

## 3. To firkanter med hver sin fart

Neste steg er å legge til en firkant til:
- den beveger seg automatisk
- uavhengig av spilleren

Poenget her:
- flere objekter oppdateres i samme game loop
- vi er klare for å snakke om kollisjon

---

## 4. Kollisjon mellom firkanter (AABB)

### Hva er AABB?

**AABB** står for **Axis-Aligned Bounding Box**.

- *Axis-aligned* betyr at firkantene ikke er rotert  
- *Bounding box* betyr at vi bruker en enkel firkant rundt objektet

I praksis:
- vi sjekker om to vanlige rektangler overlapper hverandre
- dette er standard kollisjonsmetode i enkle 2D-spill

### Prinsipp for kollisjon

To firkanter kolliderer **ikke** hvis:
- den ene er helt til venstre for den andre
- eller helt til høyre
- eller helt over
- eller helt under

Hvis ingen av disse stemmer, har vi kollisjon.

### Kollisjonsfunksjon

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

### Bruk i spillet

I game loop-en:
- sjekk kollisjon mellom spiller og den andre firkanten
- ved kollisjon → *game over* (f.eks. stoppe animasjonen og vise tekst)

---

## 5. Overgang til Flappy Bird

Nå bygger vi direkte videre på `demo8`.

### 5.1 Space = hopp

Vi bruker **samme boolean-mønster** som for piltastene.

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
- gravitasjonen trekker den ned igjen

Sprett i bakken fjernes.
Når figuren treffer bakken → *game over*.

---

### 5.2 Stolper og kollisjon

- En stolpe er også bare en firkant (egentlig to)
- De beveger seg mot spilleren
- Kollisjonsfunksjonen fra tidligere gjenbrukes

Ved kollisjon med stolpe → *game over*.

---

### 5.3 Flere stolper og enkel refaktor (hvis tid)

Hvis det er tid:
- samle stolpene i et array
- løkke gjennom arrayet
- samme kollisjonskode brukes for alle

---

## Fokus for økten

- Tastaturinput styrer fart, ikke bare posisjon
- Flere objekter kan oppdateres samtidig
- Kollisjon avgjør når spillet er over
- Flappy Bird er nå bare en kombinasjon av:
  - gravitasjon
  - hopp
  - hindringer
  - kollisjon
