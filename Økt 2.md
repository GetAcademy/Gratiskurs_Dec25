# 🧭 Økt 2 – Variabler, operatorer og animasjon med requestAnimationFrame

**Tid:** ca. 1,5 time  
**Struktur:** tre deler × ca. 25 minutter + pauser  
**Dato:** torsdag 4. desember kl. 14:00 – 15:30  

---

## 🗓️ Disposisjon

### Del 1 (0 – 25 min) – Variabler og operatorer

**Mål:** Forstå variabler, verdier og enkle operasjoner.

#### Hva er en variabel?
- En “boks” hvor vi lagrer en verdi.
- Vi kan lese og skrive verdien.

```js
let x = 50;
let y = 100;
let fart = 5;
```

#### Endring av variabler
```js
x = x + 5;   // flytt 5 piksel til høyre
y = y - 3;   // flytt 3 piksel opp
```

Kortversjoner:

```js
x += 5;
y -= 3;
```

#### Aritmetiske operatorer
- `+` pluss  
- `-` minus  
- `*` ganger  
- `/` delt  

Eksempel:

```js
let radius = 20;
let diameter = radius * 2;
```

Vis gjerne effekten på canvas:

```js
ctx.fillRect(x, 100, 100, 80);
```

Flytt `x`, lagre, oppdater → de ser det fysisk på skjermen.

---

### Del 2 (25 – 50 min) – requestAnimationFrame + bevegelse

**Mål:** Introdusere enkel animasjon ved å tegne mange ganger i sekundet.

#### requestAnimationFrame – hva og hvorfor?
- En funksjon som ber nettleseren kjøre koden vår ca. 60 ganger i sekundet.
- Brukes til alt av animasjon i canvas.

##### Eksempel: en boks som faller

```js
let y = 50;
let fart = 2;

function loop() {
  ctx.clearRect(0, 0, c.width, c.height);

  ctx.fillStyle = "blue";
  ctx.fillRect(100, y, 80, 80);

  y += fart;      // flytt boksen nedover

  requestAnimationFrame(loop);
}

loop();
```

**Poeng:**
- Vi sletter skjermen først
- Tegner på nytt
- Flytter litt
- Kaller `loop()` igjen

Dette gir illusjonen av bevegelse.

---

### Del 3 (50 – 75 min) – if-setninger + enkel kollisjon

**Mål:** Introdusere betingelser knyttet til spilllogikk.

#### if-setning – hjertet i all logikk

```js
if (y > 500) {
  fart = -2;
}
```

Forklar:
- “Hvis dette er sant → gjør dette.”
- Flappy Bird, spretterball, hinder – alt trenger `if`.

#### Eksempel: Sprett mot gulvet

```js
let y = 50;
let fart = 3;

function loop() {
  ctx.clearRect(0, 0, c.width, c.height);

  ctx.fillStyle = "red";
  ctx.fillRect(100, y, 50, 50);

  y += fart;

  // sprett
  if (y > 500) {
    fart = -3;
  }

  if (y < 0) {
    fart = 3;
  }

  requestAnimationFrame(loop);
}

loop();
```

---

## 📸 Bonus: Tegne bilder med onload (nå når de kan funksjoner)

Økt 1 rakk ikke vise bilder fordi `img.onload = ...` krever en funksjon.  
Nå kan vi ta det.

### Slik gjør du det:

```html
<img id="bird" src="bird.png" style="display:none" />
```

```js
function drawImageDemo() {
  const img = document.getElementById("bird");

  img.onload = function() {
    ctx.drawImage(img, 50, 50);
  };
}

drawImageDemo();
```

**Poeng:**
- Bildet må være lastet før `drawImage()` brukes.
- `onload` er bare en funksjon som kjører når bildet er klart.

(Valgfritt: last ned en gratis PNG fra nettet.)

---

## 📎 Ressurser
- **Discord:** (lenke deles i timen)  
- **Oppgaver:** i samme GitHub-repo som Økt 1  
- **GitHub:** https://github.com/GetAcademy/Gratiskurs_Dec25  

---

## ✏️ Oppgaver etter økten
**Anbefalte småoppgaver (samme form som i Økt 1):**
1. Beveg en firkant i ønsket retning  
2. Endre fart og retning basert på if-setninger  
3. Lag en enkel ball som spretter  

---

## ⏱️ Tidsestimat

| Del | Tema | Estimat |
|-----|------|---------|
| 1 | Variabler og operatorer | 25 min |
| 2 | requestAnimationFrame + bevegelse | 25 min |
| 3 | if-setninger + enkel kollisjon | 25 min |
| Bonus | Bilder + onload | 5 min (kan skyves til slutt) |
