# 🧭 Økt 2 – Variabler, animasjon, logikk og enkel fysikk

**Tid:** ca. 1,5 time  
**Struktur:** tre deler × ca. 25 minutter + pauser  
**Dato:** torsdag 4. desember kl. 14:00 – 15:30  

---

## 🗓️ Disposisjon

1. Variabler (globale, uten å nevne det ennå)  
2. Aritmetiske operatorer  
3. Forskjellen mellom lokale og globale variabler  
4. requestAnimationFrame + enkel animasjon uten if  
5. Logiske verdier (true/false) – f.eks. confirm()  
6. Sammenligningsoperatorer  
7. If-setninger  
8. Forbedret animasjon: gravitasjon og sprett i bakken  
9. Introduksjon av funksjoner med returverdi: `isOnGround(y)`

---

# Del 1 (0–25 min) – Variabler, operatorer og lokal vs global

## Variabler

En variabel er en boks som lagrer en verdi.

```js
let x = 100;
let y = 50;
let fart = 3;
```

Endre en variabel:

```js
x = x + 10;
y -= 5;
```

---

## Aritmetiske operatorer

```js
let a = 10 + 5;   // 15
let b = 10 - 3;   // 7
let c = 4 * 5;    // 20
let d = 20 / 4;   // 5
```

**Viktige poeng:**
- Animasjon lages ved å endre tall litt og litt.

---

## Forskjellen mellom lokale og globale variabler

**Globale variabler:**
- Lages utenfor funksjoner.  
- Initialiseres når siden lastes.  
- Lever like lenge som siden kjører.  
- Kan brukes av alle funksjoner.

```js
let y = 100; // global
```

**Lokale variabler:**
- Lages inne i funksjoner.  
- Initialiseres når funksjonen kalles.  
- Forsvinner når funksjonen er ferdig.  

```js
function test() {
  let y = 200; // lokal – eksisterer bare inne i funksjonen
}
```

---

# Del 2 (25–50 min) – requestAnimationFrame + enkel animasjon + logikk

## requestAnimationFrame – grunnmønster

```js
function loop() {
  // oppdater
  // tegn

  requestAnimationFrame(loop);
}

loop();
```

**Viktige poeng:**
- Nettsiden tegner 60 ganger i sekundet.  
- Vi endrer variabler litt hver gang → bevegelse.

---

## Eksempel: enkel animasjon uten if

```js
let x = 50;

function loop() {
  ctx.clearRect(0, 0, c.width, c.height);

  ctx.fillStyle = "blue";
  ctx.fillRect(x, 100, 60, 60);

  x += 2; // flytt litt

  requestAnimationFrame(loop);
}

loop();
```

---

## Logiske verdier – true og false

Eksempel: `confirm()` returnerer enten `true` eller `false`.

```js
let skalHoppe = confirm("Skal vi hoppe?");
```

---

## Sammenligningsoperatorer

Sammenligninger lager logiske verdier.

```js
x > 100
x < 50
x >= 200
x === 100   // lik
x !== 0     // ikke lik
```

---

## If-setninger

```js
if (x > 200) {
  ctx.fillStyle = "red";
} else {
  ctx.fillStyle = "green";
}
```

**Viktige poeng:**
- If sjekker en betingelse som må være true eller false.

---

# Del 3 (50–75 min) – Gravitasjon, sprett og returverdi

## Enkel fysikk: gravitasjon

Variabler:

```js
let y = 50;        // posisjon
let vy = 0;        // fart (velocity)
const GRAVITY = 0.4;
const GROUND = 550;
```

Animasjon:

```js
function loop() {
  ctx.clearRect(0, 0, c.width, c.height);

  vy = vy + GRAVITY; // gravitasjon øker farten nedover
  y = y + vy;

  ctx.fillStyle = "orange";
  ctx.fillRect(100, y, 50, 50);

  requestAnimationFrame(loop);
}

loop();
```

---

## Sprett i bakken (if)

```js
if (y > GROUND) {
  y = GROUND;
  vy = -vy * 0.7; // sprett med energitap
}
```

**Viktige poeng:**
- Gravitasjon endrer farten.  
- If stopper oss fra å falle gjennom bakken.  
- Sprett gjøres ved å snu farten.

---

## Funksjon med returverdi: `isOnGround(y)`

En funksjon kan gi et svar tilbake med `return`.

```js
function isOnGround(y) {
  return y >= GROUND;
}
```

Bruk i animasjonen:

```js
function loop() {
  ctx.clearRect(0, 0, c.width, c.height);

  vy += GRAVITY;
  y += vy;

  if (isOnGround(y)) {
    y = GROUND;
    vy = -vy * 0.7; // sprett
  }

  ctx.fillStyle = "purple";
  ctx.fillRect(100, y, 50, 50);

  requestAnimationFrame(loop);
}

loop();
```

**Viktige poeng:**
- `isOnGround(y)` returnerer true eller false.  
- If kan bruke returverdien direkte.  
- Gir ryddigere kode.  
- Dette er begynnelsen på “spilllogikk”.

---

## Bonus: Bilder med onload

```html
<img id="bird" src="bird.png" style="display:none">
```

```js
function drawBird() {
  const img = document.getElementById("bird");

  img.onload = function() {
    ctx.drawImage(img, 50, 50);
  };
}

drawBird();
```

**Viktige poeng:**
- Bildet må være ferdig lastet før vi tegner det.  
- `onload` kjører en funksjon når bildet er klart.

---

# 📎 Ressurser

- **Discord:** (deles i Økt 1 og gjelder for hele kurset)  
- **Oppgaver etter Økt 2:** ligger i GitHub  
- **Repo:** https://github.com/GetAcademy/Gratiskurs_Dec25  

---

# ⏱️ Tidsestimat

| Del | Tema | Estimat |
|-----|------|---------|
| 1 | Variabler, operatorer, lokal/global | 25 min |
| 2 | requestAnimationFrame, animasjon, logikk | 25 min |
| 3 | Gravitasjon, if, returverdi | 25 min |
| Bonus | Bilder med onload | 5 min |

