# 🧭 Økt 1 – Introduksjon og enkel tegning i JavaScript

**Tid:** ca. 1,5 time  
**Struktur:** tre deler × ca. 25 minutter + pauser  
**Dato:** tirsdag 2. desember kl. 14:00 – 15:30  

---

## 🗓️ Disposisjon

### Del 1 (14:00 - ca. 14:25) – Introduksjon og oppstart

**Mål:** Bli kjent, forstå hvordan kurset fungerer og få alt installert.

#### Velkommen
- Presentasjon av lærerne: **Anita**, **Geir**, **Martin** og **Terje** 
- Kort om kurset:
  - 2 uker, 4 økter á 1,5 t.  
  - Alt tas opp → lenker på **Discord** og i **GitHub-repoet**  
    - Repo: https://github.com/GetAcademy/Gratiskurs_Dec25
  - Oppfølging med Anita, Geir og Martin

#### Praktisk informasjon
- Det er **lov å ha kamera av**, men hyggelig om noen har det på.  
- **Discord:** brukes til oppfølging, spørsmål, lenker til opptak og oppgaver.  
- Opplegg: Se → forstå → spør – **ikke prøv å kode parallelt**.  

#### Installasjon og verktøy
1. **Last ned VS Code:**  
   https://code.visualstudio.com/download  
2. **Installer utvidelsen “Live Server”.**  
3. **Test:** Åpne en enkel HTML-fil og velg *Go Live*.  
4. Bruk nettleser (Chrome/Edge eller Firefox).  

#### Demo 1 - Tegne en firkant
Forklar at dette er malen som alltid kan gjenbruke.

```html
<!doctype html>
<html>
  <style>
    canvas { background-color: gray; }
  </style>
  <body>
    <canvas id="game" width="800" height="800"></canvas>
    <script>
      const c = document.getElementById("game");
      const ctx = c.getContext("2d");

      ctx.fillStyle = "red";
      ctx.fillRect(50, 50, 100, 60);   // firkant
    </script>
  </body>
</html>
```

#### Forklare alle linjene i programmet 

 - Vi går gjennom alle delene av eksemplet og forklarer. 
 - Vi tegner flere firkanter - med variasjon i:
    - posisjon
    - størrelse
    - farge
- Vi ser på hva rekkefølgen av kommandoene har å si
- Hva kan gå feil? Forhåpentligvis gjør jeg noen feil 😅 - hvis ikke så må vi konstruere noen feil og se hva som skjer da. 
  - Stavefeil  → "is not defined" i konsollen.  
  - Glemte parenteser → funksjonen kjører ikke (`drawBox` vs `drawBox()`).  
  - Feil rekkefølge på kode → tegninger havner under/over hverandre uventet.  
  - Jobbemetode: Åpne konsoll → les første feilmelding → klikk linjenummer → rett → refresh.  

---

### Del 2 (25 – 50 min) – Canvas og grunnleggende tegning

**Mål:** Forstå grunnstrukturen i HTML + JavaScript, og lære å tegne i canvas.

#### Tegne med JavaScript

Ulike måter å tegne på (full oversikt på https://www.w3schools.com/jsref/api_canvas.asp): 
- linjer
    ```js
    ctx.beginPath();
    ctx.moveTo(0, 0);
    ctx.lineTo(200, 100);
    ctx.stroke();
    ```
- sirkler og sirkelbuer
    ```js
    ctx.beginPath();
    ctx.arc(95, 50, 40, 0, 2 * Math.PI);
    ctx.stroke();
    ```
- tekst
    ```js
    ctx.font = "30px Arial";
    ctx.fillText("Hello World", 10, 50);
    ctx.strokeText("Hello World", 10, 50);
    ```
- gradient
    ```js
    // Create gradient
    var grd = ctx.createLinearGradient(0, 0, 200, 0);
    grd.addColorStop(0, "red");
    grd.addColorStop(1, "white");

    // Fill with gradient
    ctx.fillStyle = grd;
    ctx.fillRect(10, 10, 150, 80);
    ```
- bilde
    ```html
    <img src="https://upload.wikimedia.org/wikipedia/commons/7/79/Operation_Upshot-Knothole_-_Badger_001.jpg" style="display: none"/>
    ```
    ```js
    const img = document.getElementById('bildeId');
    ctx.drawImage(img, 10, 10);
    ```
    - ev. laste ned bilde + https://www.remove.bg/ + legge i mappen

#### Eksempel
Vis hvordan vi “snakker til canvas” gjennom `ctx`:

```js
ctx.fillStyle = "red";
ctx.fillRect(50, 50, 100, 60);   // firkant

ctx.beginPath();
ctx.arc(200, 150, 40, 0, Math.PI * 2); // sirkel
ctx.fillStyle = "blue";
ctx.fill();

ctx.moveTo(0, 0);
ctx.lineTo(400, 300);
ctx.stroke();                     // linje

ctx.fillText("Hei canvas!", 140, 280);
```

---

### Del 3 (50–75 min) – Funksjoner og egne kommandoer

**Mål:**  
- Forstå hva en funksjon er  
- Lage egne kommandoer  
- Introdusere parametre én etter én  
- Avslutte med en funksjon som tegner et mer komplekst objekt (ansikt)
- Feilsøking

---

# 🧩 Trinn 1 – Enkel funksjon (uten parametre, fast gjennomsiktig farge)

Vi viser at en funksjon gjør det samme hver gang den kalles.

```js
function drawBox() {
  ctx.fillStyle = "rgba(0, 255, 255, 0.4)"; // gjennomsiktig turkis
  ctx.fillRect(100, 100, 120, 80);
}

// Kall funksjonen flere ganger:
drawBox();
drawBox();
drawBox();
```

**Poenger:**
- En funksjon er en egen kommando vi lager selv.
- Hver gang vi kaller den, kjøres den samme “oppskriften”.
- Dette gjør koden ryddig og forutsigbar.

---

# 🧩 Trinn 2 – Legge til én parameter: x-posisjon

Vi lar brukeren bestemme hvor figuren havner horisontalt.

```js
function drawBox(x) {
  ctx.fillStyle = "rgba(0, 255, 255, 0.4)";
  ctx.fillRect(x, 100, 120, 80);
}

drawBox(50);
drawBox(200);
drawBox(350);
```

**Poeng:**  
- Funksjonen gjør fortsatt det samme, men du styrer *hvor*.

---

# 🧩 Trinn 3 – To parametre: x og y

Nå kan firkanten plasseres hvor som helst på skjermen.

```js
function drawBox(x, y) {
  ctx.fillStyle = "rgba(0, 255, 255, 0.4)";
  ctx.fillRect(x, y, 120, 80);
}

drawBox(50, 50);
drawBox(200, 120);
drawBox(350, 250);
```

**Poeng:**  
- Én funksjon kan tegne mange forskjellige varianter av “samme ting”.
- Funksjoner med parametre = fleksible funksjoner.

---

# 🧩 Trinn 4 – Legge til farge som parameter

Nå styrer vi ikke bare posisjon, men også utseendet.

```js
function drawBox(x, y, color) {
  ctx.fillStyle = color; // fargen bestemt av parameteren
  ctx.fillRect(x, y, 120, 80);
}

drawBox(50, 50, "red");
drawBox(200, 120, "green");
drawBox(350, 250, "rgba(0, 0, 255, 0.5)"); // blå med gjennomsiktighet
```

**Poenger:**  
- Funksjonen kan nå gjøre “det samme”, men på **mange måter**.
- Vi har nå full kontroll over posisjon *og* farge.

---

# 🧩 Trinn 5 – Funksjon som tegner noe mer komplekst: et enkelt ansikt

Her bruker vi funksjoner til å bygge en liten figur.

```js
function drawFace(x, y) {
  // Hode
  ctx.beginPath();
  ctx.fillStyle = "#ffcc99";
  ctx.arc(x, y, 60, 0, Math.PI * 2);
  ctx.fill();

  // Venstre øye
  ctx.beginPath();
  ctx.fillStyle = "black";
  ctx.arc(x - 20, y - 15, 8, 0, Math.PI * 2);
  ctx.fill();

  // Høyre øye
  ctx.beginPath();
  ctx.arc(x + 20, y - 15, 8, 0, Math.PI * 2);
  ctx.fill();

  // Munn
  ctx.beginPath();
  ctx.strokeStyle = "black";
  ctx.lineWidth = 4;
  ctx.arc(x, y + 10, 25, 0, Math.PI);
  ctx.stroke();
}

drawFace(150, 150);
drawFace(400, 200);
```

**Poenger:**
- En funksjon kan inneholde mange tegnekommandoer.
- Hele “ansiktet” kan flyttes med bare x og y.
- Dette er grunnlaget for å lage karakterer i spill.

---

# 🧩 Trappetrinn 6 – DRY: Don’t Repeat Yourself  
### Lage egne funksjoner for hode, øye og munn

Nå viser vi hvorfor funksjoner virkelig er nyttige:  
Vi slipper å gjenta den samme koden flere ganger.

I stedet for én stor `drawFace`-funksjon, deler vi opp i små byggeklosser.

---

## 6a) Funksjon for å tegne et hode

```js
function drawHead(x, y) {
  ctx.beginPath();
  ctx.fillStyle = "#ffcc99";
  ctx.arc(x, y, 60, 0, Math.PI * 2);
  ctx.fill();
}
```

---

## 6b) Funksjon for å tegne ett øye

Her er poenget: **samme funksjon tegner begge øynene**, forskjellen ligger bare i parameterne.

```js
function drawEye(x, y) {
  ctx.beginPath();
  ctx.fillStyle = "black";
  ctx.arc(x, y, 8, 0, Math.PI * 2);
  ctx.fill();
}
```

Vi kaller den to ganger med forskjellige koordinater:

```js
drawEye(x - 20, y - 15); // venstre øye
drawEye(x + 20, y - 15); // høyre øye
```

---

## 6c) Funksjon for å tegne en munn

```js
function drawMouth(x, y) {
  ctx.beginPath();
  ctx.strokeStyle = "black";
  ctx.lineWidth = 4;
  ctx.arc(x, y + 10, 25, 0, Math.PI);
  ctx.stroke();
}
```

---

## 6d) Ny versjon av drawFace – nå med gjenbruk

Nå setter vi sammen komponentene til én figur:

```js
function drawFace(x, y) {
  drawHead(x, y);
  drawEye(x - 20, y - 15);
  drawEye(x + 20, y - 15);
  drawMouth(x, y);
}

drawFace(150, 150);
drawFace(400, 200);
```

---

## Hvorfor er dette bedre?

- Hvis vi vil endre fargen på øynene → endrer vi **ett sted**.  
- Hvis vi vil gjøre hodet større → endrer vi **ett sted**.  
- Hvis vi vil bruke øyne til et annet prosjekt → vi kan gjenbruke funksjonen.  
- Funksjonene blir små, enkle og gjør **én ting hver**.  
- `drawFace()` blir ren og lett å lese, nesten som en setning:

```
Tegn hode  
Tegn venstre øye  
Tegn høyre øye  
Tegn munn  
```

Dette er kjernen i **DRY**:  
> “Don’t Repeat Yourself” – skriv ting *ett sted*, bruk det mange ganger.

---

Neste steg i kurset:  
I Økt 2 bruker vi denne idéen til å lage **bevegelige** figurer!


# 🧩 Oppsummering

I denne delen lærte dere:

- Hva funksjoner er og hvorfor vi lager dem  
- Hvordan vi kan starte enkelt og bygge opp kompleksitet  
- Hvordan parametre gir oss mer kontroll  
- Hvordan vi kan tegne en hel figur med én funksjon  

Neste gang begynner vi med **bevegelse og animasjon** – ting skal røre på seg!
