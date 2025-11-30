## Økt 1

### Oppgave 1 + installer VScode og live server

Windows:
1) Åpne nettleser og gå til [Visual Studio Code](https://code.visualstudio.com/Download)
2) Last ned program og følg instruksjonene
3) Åpne Visual Studio Code - konfigurer det slik du måtte ønske (eller bare klikk deg videre)
4) Åpne "Extensions" (fire firkanter til venstre i VSCode vinduet) 
    
    ![](./img/extension%20icon.png)
5) Søk opp "Live Server" og last ned Live Server av "Ritwick Dey"

### Hvis det oppstår noen problemer videre...

Fikk du noe problemer når du lasta ned Visual Studio Code, eller ser du ikke resultatet i nettleser... eller noe annet du lurer på?
Ta kontakt med Geir eller Martin!

---

### Oppgave 2 - Ditt første canvas-program

**Målsetning:** Lag en HTML-fil med canvas og tegn en enkel form

**Steg-for-steg guide:**

1. **Opprett en ny fil**
   - Åpne Visual Studio Code
   - Lag en ny mappe til koden
   - Lag en ny fil ved å trykke `Ctrl+N` (eller `Cmd+N` på Mac)
   - Lagre filen som `tegning.html` (Ctrl+S)

2. **Skriv HTML-preambelen**

   ```html
   <!DOCTYPE html>
   <html lang="no">
   <head>
       <meta charset="UTF-8">
       <title>Min første tegning</title>
   </head>
   <body>
        <h1>Hello world!</h1>
   </body>
   </html>
   ```
    2.1. **Høyreklikk på "tegning.html" eller inne i editor og velg "Open with Live Server"** ("Hello world!" burde vise seg i nettleser!)

    2.2. **Prøv å bytt ut "world" med navnet ditt og trykk CTRL+S (lagre)**

    2.3. **Nettsiden burde vise "Hello (navn)!" automatisk!**

    (?)Terje? -> **Bonus:** **Prøv med noen andre HTML-tags** - [headings, p, hr her, f.eks.!](https://www.w3schools.com/tags/ref_byfunc.asp)

3. **Legg til canvas-elementet**
   - Inni `<body>`-taggen, skriv:

   ```html
   <canvas id="minCanvas" width="400" height="300"></canvas>
   ```

4. **Legg til et script-tag**
   - Før `</body>`-slutt-taggen og etter canvas taggen, skriv:

   ```html
   <script>
       // Din JavaScript-kode kommer her
   </script>
   ```

5. **Hent canvas og kontekst**
   - Putt canvas og kontekst i script-taggen:

   ```js
   <script>
        const canvas = document.getElementById('minCanvas');
        const ctx = canvas.getContext('2d');
   </script>
   ```

6. **Tegn en firkant**
   - Rett etter konteksten, skriv:

   ```javascript
   ctx.fillStyle = 'blue';
   ctx.fillRect(50, 50, 150, 100);
   ```

7. **Åpne filen i nettleseren**
    
    Hvis ikke du har åpna Live Server allerede:
   - Høyreklikk på filen i VS Code
   - Velg "Open with Live Server"
   - Du skal nå se en blå firkant!

---

### Ekstraoppgaver:

- Tegn flere firkanter med ulike farger
- Tegn en sirkel ved å bruke `ctx.arc()` og `ctx.fill()`
    
    ```js
    // Dette tegner sirkel (x, y, radius, startAngle, endAngle)
    // En "startAngle" på "0" lager full sirkel
    ctx.arc(100, 100, 30, 0, Math.PI * 2);
    // Putter penselen til gul
    ctx.fillStyle = "yellow";
    // Maler innsida på sirkelen gul
    ctx.fill();
    // Lager en linje rundt sirkelen
    ctx.stroke();
    ```
- Forandre på tallene i arc() eller fargen på fillStyle - sjekk ut hva som skjer!
- Tegn en linje ved å bruke `ctx.moveTo()` og `ctx.lineTo()`

    ```js
    ctx.moveTo(100,100); // x og y koordinater - setter et punkt på canvas
    ctx.lineTo(200,200); // setter sluttpunktet på canvas
    ctx.stroke(); // tegner linja
    ```
- Prøv selv; **Lag to linjer som danner en "X"!**

**Bonus:**
- Tegn en Microsoft Windows logo!
- ![](./img/microsoft.png)
1) Lag fire firkanter som Windows logoen med riktig posisjon og farge
2) Legg til teksten "Microsoft" under (Hint: [fillText()](https://www.w3schools.com/tags/canvas_filltext.asp), [font](https://www.w3schools.com/TAgs/canvas_font.asp))

**EXTRA SUPER bonus:**
- Tegn en Pacman!!!
- ![](./img/pacman.png)

Forslag til fremgangsmåte:
1) Lag en sirkel med `arc()` og `stroke()`
2) Lag en linje med `lineTo()` inn til sentrum av sirkelen (kobler opp "gapet") - flytt `stroke()` til under denna linja!
3) Gjør `startAngle`-tallet til et litt større komma-tall, og `endAngle`-tallet til noe litt mindre komma-tall (endrer "gapet" til pacman)
4) Kan hende du trenger enda en `lineTo()` for å sette andre delen av gapet
5) Fyll dette med en gul farge (`fillStyle`, `fill()`)
6) Lag flere små sirkler foran gapet! (Kjør en `ctx.beginPath()` etter koden over og før hver nye sirkel, slik at det ikke blir noen rare linjer over skjermen)
