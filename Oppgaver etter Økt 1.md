# Økt 1

## Oppgave 1 + installer VScode og live server

Windows:

1) Åpne nettleser og gå til [Visual Studio Code](https://code.visualstudio.com/Download)
2) Last ned program og følg instruksjonene
3) Åpne Visual Studio Code - konfigurer det slik du måtte ønske (eller bare klikk deg videre)
4) Åpne "Extensions" (fire firkanter til venstre i VSCode vinduet)

    ![extension icon](./img/extension%20icon.png)
5) Søk opp "Live Server" og last ned Live Server av "Ritwick Dey"

### Hvis det oppstår noen problemer videre

Fikk du noe problemer når du lasta ned Visual Studio Code, eller ser du ikke resultatet i nettleser... eller noe annet du lurer på?
Ta kontakt med Geir eller Martin!

---

## Oppgave 2 - Ditt første canvas-program

**Målsetning:** Lag en HTML-fil med canvas og tegn en enkel form

1. **Opprett en ny fil**
   - Åpne Visual Studio Code
   - Lag en ny mappe til koden
   - Lag en ny fil ved å trykke `Ctrl+N` (eller `Cmd+N` på Mac)
   - Lagre filen som `tegning.html` (Ctrl+S)
2. **Lim inn denne koden**:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Van Gogh</title>
    <style>
        canvas {
            border: 1px solid black;
        }
    </style>
</head>
<body>
    <canvas id="canvas" width="800" height="600"></canvas>

    <script>
        const c = document.getElementById('canvas');
        const ctx = c.getContext('2d');

        // Skriv kode her! ↓↓↓
        

    </script>
</body>
</html>

```
3. **Høyreklikk på `tegning.html` i "Explorer" (eller høyreklikk inne på dokumentet) og trykk på `Open with Live Server`**
    - Nettleseren burde dukke opp på skjermen
    - Den burde vise en svart firkant (vårt lerret!)
4. **Tegn en firkant!**
    - Rett under den grønne kommentaren hvor det står "Skriv kode her!", skriv:
    ```js
    ctx.fillStyle = 'blue'
    ctx.fillRect(50,50,150,100)
    ```
    - Lagre filen og gå tilbake til nettleser - en blå firkant burde dukke opp på skjermen!


## Ekstraoppgaver

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

### Bonus:

- Tegn en Microsoft Windows logo!
- ![microsoft logo](./img/microsoft.png)

1) Lag fire firkanter som Windows logoen med riktig posisjon og farge
2) Legg til teksten "Microsoft" under (Hint: [fillText()](https://www.w3schools.com/tags/canvas_filltext.asp), [font](https://www.w3schools.com/TAgs/canvas_font.asp))

### EXTRA SUPER bonus:
- Tegn en Pacman!!!
- ![pacman](./img/pacman.png)

**Forslag til fremgangsmåte:**

1. **Lag en sirkel med `arc()` og `stroke()`**
    <details>
      <summary>👈 Klikk for et forslag</summary>

      ```js
      ctx.arc(200,200,50,0,2*Math.PI)
      ctx.stroke()
      ```
    </details>
    <br>

2. **Lag en linje med `lineTo()` inn til sentrum av sirkelen (kobler opp "gapet") - flytt `stroke()` under denne linja for å tegne opp!**
    <details>
      <summary>👈 Klikk for et forslag</summary>

      ```js
      ctx.arc(200, 200, 50, 0, 2*Math.PI)
      ctx.lineTo(200, 200)
      ctx.stroke()
      ```
    </details>
    <br>

3. **Gjør `startAngle`-tallet til sirkelen til et litt *større* kommatall, og `endAngle`-tallet til et litt *mindre* kommatall (endrer "gapet" til Pacman)**
    <details>
      <summary>👈 Klikk for et forslag</summary>

      ```js
      ctx.arc(200, 200, 50, 0.6, 1.8*Math.PI)
      ctx.lineTo(200, 200)
      ctx.stroke()
      ```
    </details>
    <br>

4. **Sett enda en `lineTo()` for å koble opp gapet**
    <details>
      <summary>👈 Klikk for et forslag</summary>

      ```js
      ctx.arc(200, 200, 50, 0.6, 1.8*Math.PI)
      ctx.lineTo(200, 200) 
      ctx.lineTo(242, 230) 
      ctx.stroke()
      ```
    </details>
    <br>

5. **Fyll Pacman med gul farge! (`fillStyle`, `fill()`)**
    <details>
      <summary>👈 Klikk for et forslag</summary>

      ```js
      ctx.arc(200,200,50,0.6,1.8*Math.PI);
      ctx.lineTo(200,200);
      ctx.lineTo(242,230)
      ctx.fillStyle = "yellow"
      ctx.fill()
      ctx.stroke();
      ```
    </details>
    <br>

6. **Lag flere små sirkler foran gapet! (hint: Prøv [`beginPath()`](https://www.w3schools.com/jsref/canvas_beginpath.asp) før hver sirkel for å unngå streker over skjermen)**
    <details>
      <summary>👈 Klikk for et forslag</summary>

      ```js
      ctx.arc(200,200,50,0.6,1.8*Math.PI);
      ctx.lineTo(200,200);
      ctx.lineTo(242,230)
      ctx.fillStyle = "yellow"
      ctx.fill()
      ctx.stroke();

      ctx.beginPath()
      ctx.arc(300,200,5,0,2*Math.PI)
      ctx.stroke()

      ctx.beginPath()
      ctx.arc(400,200,5,0,2*Math.PI)
      ctx.stroke()

      ctx.beginPath()
      ctx.arc(500,200,5,0,2*Math.PI)
      ctx.stroke()
      ```
    </details>
