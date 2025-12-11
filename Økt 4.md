# Økt 4 – Score, restart og helhet i spillet
**JavaScript Canvas – fullføre Flappy Martin**

I denne økten fullfører vi spillet vi har bygget så langt.
Fokuset er ikke nye konsepter, men å:
- gjøre spillet komplett
- forstå hva *state* faktisk er
- rydde opp nok til at koden er forståelig

Økten er delt i tre bolker á ca. 25 minutter, med god tid til spørsmål.

---

## Men først litt info om GET Academy

- www.getacademy.no

## Oversikt over økten

1. Game over som tydelig tilstand  
2. Score  
3. Restart + spørsmål / videre utforsking  

---

## Del 1 – Game over som tilstand (ca. 25 min)

Vi starter med koden slik den er etter forrige økt.
Spillet stopper når vi krasjer, men logikken er litt spredt.

Målet nå er å gjøre **game over til en eksplisitt tilstand**.

---

### 1.1 Introdusere gameOver-variabel

Øverst i koden:

```js
let gameOver = false;
```

Denne variabelen skal:
- være `false` mens spillet kjører
- bli `true` når vi krasjer i bakken eller en stolpe

---

### 1.2 Samle game over-logikk

I game loop-en:
- sjekk alle typer kollisjon på ett sted
- hvis noe treffer → `gameOver = true`

Konseptuelt:

```js
if (collisionWithGround || collisionWithPipe) {
    gameOver = true;
}
```

Dette gjør at:
- resten av koden kan forholde seg til `gameOver`
- vi slipper å stoppe animasjonen på mange forskjellige steder

---

### 1.3 Tegne Game Over-tekst

Når `gameOver` er `true`:
- vi tegner fortsatt banen
- men viser en tydelig tekst

```js
ctx.fillStyle = 'black';
ctx.font = '36px Arial';
ctx.fillText('Game Over', 120, 200);
```

Viktig poeng:
> Game over er bare en tilstand, ikke slutten på programmet.

---

## Del 2 – Score (ca. 25 min)

Nå legger vi på score.
Dette gjør spillet mer meningsfylt uten å legge til mye kompleksitet.

---

### 2.1 Hva mener vi med score?

I Flappy Martin kan score være:
- hvor lenge vi overlever
- hvor mange hindringer vi passerer

Her bruker vi **tid-overlevd** fordi det er robust og enkelt.

---

### 2.2 Score-variabel

```js
let score = 0;
```

---

### 2.3 Oppdatere score

I game loop-en:
- øk score så lenge spillet ikke er over

```js
if (!gameOver) {
    score++;
}
```

Dette gir:
- jevn score-økning
- enkel forståelse

---

### 2.4 Tegne score

Etter at bakgrunn og objekter er tegnet:

```js
ctx.fillStyle = 'black';
ctx.font = '24px Arial';
ctx.fillText('Score: ' + score, 20, 30);
```

Viktig poeng:
> Score er bare en del av game state – på linje med posisjon og fart.

---

## Del 3 – Restart (ca. 25 min)

Når spillet er over, vil vi kunne starte på nytt uten å refreshe siden.

---

### 3.1 Hva betyr restart?

Restart betyr:
- ikke magi
- ikke ny side

Restart betyr:
> Vi setter alle relevante variabler tilbake til startverdier.

---

### 3.2 Samle restart-logikk i én funksjon

```js
function restartGame() {
    gameOver = false;
    score = 0;

    x = startX;
    y = startY;
    speedY = 0;

    resetPipes();
}
```

Poenget:
- all logikk for restart er samlet
- lett å finne og forstå

---

### 3.3 Starte restart med tast

Vi gjenbruker tastestyringen vi allerede har.

For eksempel Enter-tasten:

```js
let enterPressed = false;

function handleKeyDown(event) {
    if (event.code === 'Enter') enterPressed = true;
}

function handleKeyUp(event) {
    if (event.code === 'Enter') enterPressed = false;
}
```

Når spillet er over:

```js
if (gameOver && enterPressed) {
    restartGame();
    requestAnimationFrame(drawRectangles);
}
```

---

## Avslutning – Helhet og refleksjon

Vi avslutter med spørsmål og utforsking:

- Hvilke variabler utgjør egentlig spillets state?
- Hva skjer hvis vi øker gravitasjon?
- Hva skjer hvis vi gjør gapet mellom stolpene mindre?
- Hvordan kunne vi gjort spillet vanskeligere / lettere?

Viktig poeng:
> Et spill er bare state + regler + rendering i loop.

Dette er samme tenkning vi bruker i større applikasjoner også.
