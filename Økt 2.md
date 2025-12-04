# 🧭 Økt 2 – Variabler, animasjon, enkel fysikk og if-setninger

**Tid:** ca. 1,5 time  
**Struktur:** tre deler × ca. 25 minutter + pauser  
**Dato:** torsdag 4. desember kl. 14:00 – 15:30  

---

## 🗓️ Disposisjon

- Kort repetisjon fra økt 1  
- Globale og lokale variabler  
- Aritmetiske og sammenligningsoperatorer  
- requestAnimationFrame og animasjon  
- Enkel fysikk med gravitasjon  
- If-setninger og returverdi  

---

### Del 1 (0–25 min) – Variabler, global vs lokal, operatorer

**Mål:** Forstå variabler bedre, og se forskjell på global og lokal variabel. Introdusere sammenligningsoperatorer.

#### Repetisjon: Hva er en variabel?

```js
let x = 50;
let y = 100;
let fart = 3;
```

- En “boks” vi lagrer en verdi i.
- Vi kan endre verdien:

```js
x = x + 10;
y -= 5;        // kortversjon for y = y - 5;
```

#### Globale vs lokale variabler

**Global variabel** – definert utenfor funksjoner, kan brukes “over alt”:

```js
const c = document.getElementById("canvas");
const ctx = c.getContext("2d");

let y = 100; // global

function draw() {
  ctx.fillStyle = "blue";
  ctx.fillRect(100, y, 80, 80);
}
```

**Lokal variabel** – definert inne i en funksjon, finnes bare der:

```js
function drawLocalBox() {
  let y = 200;  // lokal – lever bare inni funksjonen
  ctx.fillStyle = "green";
  ctx.fillRect(200, y, 80, 80);
}
```

Viktige poeng:
- Globale variabler er fine når flere funksjoner trenger samme informasjon (posisjon, fart, score).  
- Lokale variabler er fine når noe bare gjelder inni én funksjon.
- Globale variabler initialiseres når siden lastes - og lever like lenge som siden
- Lokale variabler initialiseres når funksjonen kalles - og lever inntil funksjonen har kjørt ferdig


---

#### Aritmetiske operatorer

```js
let a = 10 + 5;   // 15
let b = 10 - 3;   // 7
let c2 = 4 * 5;   // 20
let d = 20 / 4;   // 5
```

---

#### Sammenligningsoperatorer

Brukes alltid med `if`:

```js
x > 100      // større enn
x < 100      // mindre enn
x >= 100     // større enn eller lik
x <= 100     // mindre enn eller lik
x === 100    // lik (både verdi og type)
x !== 100    // ikke lik
```

Enkel demo:

```js
if (x > 200) {
  ctx.fillStyle = "red";
} else {
  ctx.fillStyle = "blue";
}
ctx.fillRect(x, 100, 80, 80);
```

---

### Del 2 (25–50 min) – requestAnimationFrame og enkel gravitasjon

**Mål:** Se hvordan vi får ting til å bevege seg, og introdusere enkel fysikk med gravitasjon.

#### requestAnimationFrame – grunnmønsteret

```js
function loop() {
  // oppdater ting
  // tegn ting

  requestAnimationFrame(loop);
}

loop();
```

#### Demo: boks som faller med gravitasjon

```js
let y = 50;          // posisjon
let vy = 0;          // fart (velocity)
const GRAVITY = 0.4; // konstant gravitasjon
const GROUND = 550;  // "bakken"

function loop() {
  ctx.clearRect(0, 0, c.width, c.height);

  // fysikk: oppdatere fart og posisjon
  vy = vy + GRAVITY; // gravitasjon trekker ned
  y = y + vy;

  // bakken – enkel stopp
  if (y > GROUND) {
    y = GROUND;
    vy = 0;
  }

  // tegn boksen
  ctx.fillStyle = "orange";
  ctx.fillRect(100, y, 50, 50);

  requestAnimationFrame(loop);
}

loop();
```

Poeng:
- `vy` (fart) endrer seg litt hver gang → akselerasjon.
- Gravitasjon er bare å legge til et lite tall på fart hver frame.
- Vi bruker en `if` for å hindre at boksen faller gjennom bakken.

---

### Del 3 (50–75 min) – If-setninger, returverdi og enkel logikk

**Mål:** Forstå if-setninger bedre, og introdusere funksjoner med returverdi i en meningsfull situasjon.

#### If-setninger i praksis

Vi har allerede brukt `if` for bakken.  
Vis også et enkelt eksempel med farge:

```js
let x = 50;

function loop() {
  ctx.clearRect(0, 0, c.width, c.height);

  if (x > c.width / 2) {
    ctx.fillStyle = "red";
  } else {
    ctx.fillStyle = "blue";
  }

  ctx.fillRect(x, 100, 80, 80);

  x += 2;

  requestAnimationFrame(loop);
}

loop();
```

---

#### Funksjoner med returverdi – “svar” fra funksjonen

Vi lager en funksjon som **sjekker noe** og gir `true` eller `false` tilbake.  
Den passer rett inn i fysikken vi allerede har.

```js
const GROUND = 550;
const RADIUS = 25;

let y = 50;
let vy = 0;
const GRAVITY = 0.4;

function isOnGround(y) {
  return y + RADIUS >= GROUND;
}
```

Bruk i løkken:

```js
function loop() {
  ctx.clearRect(0, 0, c.width, c.height);

  vy += GRAVITY;
  y += vy;

  if (isOnGround(y)) {
    y = GROUND - RADIUS;
    vy = 0;
  }

  ctx.beginPath();
  ctx.arc(150, y, RADIUS, 0, Math.PI * 2);
  ctx.fillStyle = "purple";
  ctx.fill();

  requestAnimationFrame(loop);
}

loop();
```

Poeng:
- `isOnGround(y)` er en funksjon som **returnerer** `true` eller `false`.
- `return` gir oss ett “svar” som vi kan bruke i en `if`.
- Dette er mønsteret vi vil bruke hele tiden i spill-logikk.

---

### Bonus: Bilder med `onload` (nå som de kan funksjoner)

Kort demo (kan tas til slutt):

```html
<img id="bird" src="bird.png" style="display:none">
<canvas id="canvas" width="800" height="600"></canvas>
```

```js
const c = document.getElementById("canvas");
const ctx = c.getContext("2d");

function drawBirdWhenReady() {
  const img = document.getElementById("bird");

  img.onload = function() {
    ctx.drawImage(img, 50, 50);
  };
}

drawBirdWhenReady();
```

Forklaring:
- `onload = function() { ... }` betyr: “Når bildet er klart, kjør denne funksjonen.”
- Nå gir det mening fordi de har sett funksjoner og retur.

---

## 📎 Ressurser

- **Discord:** (lenke deles i timen)  
- **Oppgaver:** `Oppgaver etter Økt 2.md` i GitHub-repoet  
- **GitHub:** https://github.com/GetAcademy/Gratiskurs_Dec25  

---

## ⏱️ Tidsestimat

| Del | Tema | Estimat |
|-----|------|---------|
| 1 | Variabler, global vs lokal, operatorer | 25 min |
| 2 | requestAnimationFrame + gravitasjon | 25 min |
| 3 | If-setninger + returverdi | 25 min |
| Bonus | Bilder med onload | 5 min (dersom det er tid) |
# 🧭 Økt 2 – Variabler, animasjon, enkel fysikk og if-setninger

**Tid:** ca. 1,5 time  
**Struktur:** tre deler × ca. 25 minutter + pauser  
**Dato:** torsdag 4. desember kl. 14:00 – 15:30  

---

## 🗓️ Disposisjon

- Kort repetisjon fra økt 1  
- Globale og lokale variabler  
- Aritmetiske og sammenligningsoperatorer  
- requestAnimationFrame og animasjon  
- Enkel fysikk med gravitasjon  
- If-setninger og returverdi  

---

### Del 1 (0–25 min) – Variabler, global vs lokal, operatorer

**Mål:** Forstå variabler bedre, og se forskjell på global og lokal variabel. Introdusere sammenligningsoperatorer.

#### Repetisjon: Hva er en variabel?

```js
let x = 50;
let y = 100;
let fart = 3;
```

- En “boks” vi lagrer en verdi i.
- Vi kan endre verdien:

```js
x = x + 10;
y -= 5;        // kortversjon for y = y - 5;
```

#### Globale vs lokale variabler

**Global variabel** – definert utenfor funksjoner, kan brukes “over alt”:

```js
const c = document.getElementById("canvas");
const ctx = c.getContext("2d");

let y = 100; // global

function draw() {
  ctx.fillStyle = "blue";
  ctx.fillRect(100, y, 80, 80);
}
```

**Lokal variabel** – definert inne i en funksjon, finnes bare der:

```js
function drawLocalBox() {
  let y = 200;  // lokal – lever bare inni funksjonen
  ctx.fillStyle = "green";
  ctx.fillRect(200, y, 80, 80);
}
```

Poeng å si høyt:
- Globale variabler er fine når flere funksjoner trenger samme informasjon (posisjon, fart, score).  
- Lokale variabler er fine når noe bare gjelder inni én funksjon.

---

#### Aritmetiske operatorer

```js
let a = 10 + 5;   // 15
let b = 10 - 3;   // 7
let c2 = 4 * 5;   // 20
let d = 20 / 4;   // 5
```

---

#### Sammenligningsoperatorer

Brukes alltid med `if`:

```js
x > 100      // større enn
x < 100      // mindre enn
x >= 100     // større enn eller lik
x <= 100     // mindre enn eller lik
x === 100    // lik (både verdi og type)
x !== 100    // ikke lik
```

Enkel demo:

```js
if (x > 200) {
  ctx.fillStyle = "red";
} else {
  ctx.fillStyle = "blue";
}
ctx.fillRect(x, 100, 80, 80);
```

---

### Del 2 (25–50 min) – requestAnimationFrame og enkel gravitasjon

**Mål:** Se hvordan vi får ting til å bevege seg, og introdusere enkel fysikk med gravitasjon.

#### requestAnimationFrame – grunnmønsteret

```js
function loop() {
  // oppdater ting
  // tegn ting

  requestAnimationFrame(loop);
}

loop();
```

#### Demo: boks som faller med gravitasjon

```js
let y = 50;          // posisjon
let vy = 0;          // fart (velocity)
const GRAVITY = 0.4; // konstant gravitasjon
const GROUND = 550;  // "bakken"

function loop() {
  ctx.clearRect(0, 0, c.width, c.height);

  // fysikk: oppdatere fart og posisjon
  vy = vy + GRAVITY; // gravitasjon trekker ned
  y = y + vy;

  // bakken – enkel stopp
  if (y > GROUND) {
    y = GROUND;
    vy = 0;
  }

  // tegn boksen
  ctx.fillStyle = "orange";
  ctx.fillRect(100, y, 50, 50);

  requestAnimationFrame(loop);
}

loop();
```

Poeng:
- `vy` (fart) endrer seg litt hver gang → akselerasjon.
- Gravitasjon er bare å legge til et lite tall på fart hver frame.
- Vi bruker en `if` for å hindre at boksen faller gjennom bakken.

---

### Del 3 (50–75 min) – If-setninger, returverdi og enkel logikk

**Mål:** Forstå if-setninger bedre, og introdusere funksjoner med returverdi i en meningsfull situasjon.

#### If-setninger i praksis

Vi har allerede brukt `if` for bakken.  
Vis også et enkelt eksempel med farge:

```js
let x = 50;

function loop() {
  ctx.clearRect(0, 0, c.width, c.height);

  if (x > c.width / 2) {
    ctx.fillStyle = "red";
  } else {
    ctx.fillStyle = "blue";
  }

  ctx.fillRect(x, 100, 80, 80);

  x += 2;

  requestAnimationFrame(loop);
}

loop();
```

---

#### Funksjoner med returverdi – “svar” fra funksjonen

Vi lager en funksjon som **sjekker noe** og gir `true` eller `false` tilbake.  
Den passer rett inn i fysikken vi allerede har.

```js
const GROUND = 550;
const RADIUS = 25;

let y = 50;
let vy = 0;
const GRAVITY = 0.4;

function isOnGround(y) {
  return y + RADIUS >= GROUND;
}
```

Bruk i løkken:

```js
function loop() {
  ctx.clearRect(0, 0, c.width, c.height);

  vy += GRAVITY;
  y += vy;

  if (isOnGround(y)) {
    y = GROUND - RADIUS;
    vy = 0;
  }

  ctx.beginPath();
  ctx.arc(150, y, RADIUS, 0, Math.PI * 2);
  ctx.fillStyle = "purple";
  ctx.fill();

  requestAnimationFrame(loop);
}

loop();
```

Poeng:
- `isOnGround(y)` er en funksjon som **returnerer** `true` eller `false`.
- `return` gir oss ett “svar” som vi kan bruke i en `if`.
- Dette er mønsteret vi vil bruke hele tiden i spill-logikk.

---

### Bonus: Bilder med `onload` (nå som de kan funksjoner)

Kort demo (kan tas til slutt):

```html
<img id="bird" src="bird.png" style="display:none">
<canvas id="canvas" width="800" height="600"></canvas>
```

```js
const c = document.getElementById("canvas");
const ctx = c.getContext("2d");

function drawBirdWhenReady() {
  const img = document.getElementById("bird");

  img.onload = function() {
    ctx.drawImage(img, 50, 50);
  };
}

drawBirdWhenReady();
```

Forklaring:
- `onload = function() { ... }` betyr: “Når bildet er klart, kjør denne funksjonen.”
- Nå gir det mening fordi de har sett funksjoner og retur.

---

## 📎 Ressurser

- **Discord:** (lenke deles i timen)  
- **Oppgaver:** `Oppgaver etter Økt 2.md` i GitHub-repoet  
- **GitHub:** https://github.com/GetAcademy/Gratiskurs_Dec25  

---

## ⏱️ Tidsestimat

| Del | Tema | Estimat |
|-----|------|---------|
| 1 | Variabler, global vs lokal, operatorer | 25 min |
| 2 | requestAnimationFrame + gravitasjon | 25 min |
| 3 | If-setninger + returverdi | 25 min |
| Bonus | Bilder med onload | 5 min (dersom det er tid) |
