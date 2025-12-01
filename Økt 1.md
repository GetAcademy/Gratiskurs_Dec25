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


---

### Del 2 (25 – 50 min) – Canvas og grunnleggende tegning

**Mål:** Forstå grunnstrukturen i HTML + JavaScript, og lære å tegne i canvas.

#### Tegne med JavaScript

Ulike måter å tegne på (full oversikt på https://www.w3schools.com/html/html5_canvas.asp): 
- linjer
    ```js
    ctx.moveTo(0, 0);
    ctx.lineTo(200, 100);
    ctx.stroke();`
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

### Del 3 (50 – 75 min) – Funksjoner og egne kommandoer

**Mål:** Lære å lage og bruke funksjoner for å gjenbruke kode.

#### Demo 2 – Funksjoner uten og med parametre

```js
function drawBox() {
  ctx.fillStyle = "green";
  ctx.fillRect(100, 100, 80, 60);
}

function drawCircle(x, y, r, color) {
  ctx.beginPath();
  ctx.fillStyle = color;
  ctx.arc(x, y, r, 0, Math.PI * 2);
  ctx.fill();
}
```

#### Feilsøking og vanlige feil
- “Undefined” → variabel ikke definert.  
- “Unexpected token” → glemt parentes eller klamme.  
- Bruk nettleserkonsollen (`Ctrl + Shift + I → Console`) for feilmeldinger.

---

### Del 4 (75 – 90 min) – Oppsummering og oppgaver

**Oppgaver (frivillige mellom øktene):**
1. Endre fargene på figurer.  
2. Tegn noe eget (logo, flagg, figur).  
3. Se opptaket og prøv å gjenskape demoen.  

**Neste gang (torsdag 4. des.):**
- Variabler og operatorer  
- `requestAnimationFrame()` for bevegelse  
- Enkle `if`-setninger  
- Fysikk (fart og retning)

---

## ⏱️ Tidsestimat

| Del | Tema | Estimat |
|-----|------|---------|
| 1 | Velkommen, info, installasjon | 25 min |
| 2 | Canvas og tegning | 25 min |
| 3 | Funksjoner + feilsøking | 25 min |
| 4 | Spørsmål + oppsummering | 10 min |

---

## 📎 Ressurser

- **Discord:** (lenke deles i timen)  
- **GitHub-repo:** https://github.com/GetAcademy/SimpleFlappyBirdWeb  
- **Demo-side:** https://getacademy.github.io/SimpleFlappyBirdWeb/  
- **Neste økt:** Torsdag 4. desember kl. 14–15:30  

