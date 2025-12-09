# Økt 3
Videre nedover her skal vi teste oss litt ut på gravitasjon, masse bruk av variabler og dette med å bruke tastetrykk for å bevege noe over canvas!

## Oppgave 1: Sprettball!

[Tidligere](./Oppgaver%20etter%20Økt%202.md) har vi vært borti dette med å få en sirkel til å ikke forlate canvas med `if`-setninger og variabler. Nå skal vi teste oss litt på å lage enkel fysikk, hvor sirkelen faller og spretter i det den treffer "gulvet"!

1) Lag en ny fil
    - Kall den `sprettern.html`
    - Kopier inn denne koden og start en Live Server
    - Du burde se en svart firkant med en sirkel!
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
        <style>
            canvas {
                border: 1px solid black;
            }
        </style>
    </head>
    <body>
        <canvas id="vangogh" width="800" height="600"></canvas>

        <script>
            const c = document.getElementById('vangogh');
            const ctx = c.getContext('2d');

            // Skriv variabler her!
            let x = 50;
            let y = 50;


            move()
            function move() {
                ctx.clearRect(0,0,c.width,c.height);

                ctx.beginPath();
                ctx.arc(x,y,30,0,Math.PI*2);
                ctx.stroke();

                requestAnimationFrame(move)
            }

        </script>
    </body>
    </html>
    ```
2) Legg til to variabler for `xSpeed` og `ySpeed`
    - Sett de til noe lavt, som f.eks.
    ```js
    let xSpeed = 2
    let ySpeed = 0
    ```
    - På en ny linje i funksjonen, legg til fartsvariablen på `x` og `y`
    ```js
    x += xSpeed
    y += ySpeed
    ```
    - Sirkelen burde nå bevege seg!
3) Legg til physics!
    - Lag en ny variabel, kall den for `gravity`
    - Gi den en lav verdi, noe som `0.1`
    - Mellom `ctx.clearRect()` og `ctx.beginPath()` i funksjonen, skriv:
    ```js
    ySpeed = ySpeed + gravity
    ```
    - Sirkel burde nå falle nedover gradvis!
4) Få den til å "sprette" på bakken
    - Lag en `if`-setning i funksjonen
    - Her må vi sjekke
        - Hvis `y` er mer enn høyden til canvas (bakken)
        - -> Sett `ySpeed` til det motsatte!
    Dette kan se slik ut:
    ```js
    // Trekker fra radius på ball (-30), sånn at den ikke "klipper" gjennom gulvet
    if(y > c.height - 30) {
        ySpeed = -ySpeed
    }
    ```
    - Sirkel burde nå sprette av "gulvet"!
5) Bonus!
    - Når en ball spretter, så mister den litt energi hver gang den spretter (så den spretter ikke like høyt hver gang som den gjør nå).
    - Dette kan vi simulere:
    ```js
    // På if-setningen
    if(y > c.height - 30) {
        ySpeed = -ySpeed * 0.8 //Fjerner litt og litt energi for hver gang
    }
    ```
    - Hvis man finner at ballen blir "stuck" i gulvet, så kan det være en kjapp fix å midlertidig sette `y`-posisjonen til "gulvet" i if-setningen:
    ```js
    if(y > c.height - 30) {
        y = c.height - 30 // setter y-posisjon til "gulvet"
        ySpeed = -ySpeed * 0.8
    }
    ```
6) Test ut selv!
    - Nå spretter ballen avgårde ut på sidene
        - Lag noen if-sjekker som tar høyde for det!

## Oppgave 2: Bevegelse med tastetrykk!

Ta utgangspunkt i denna koden:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Canvas mal</title>
    <style>
        canvas {
            background-image: url("https://kompis.s-ul.eu/j5iMIAzu");
            border: 3px solid black;
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

Vi ønsker å bruke tastaturet til å bevege noe langs veien.

1) **Finn et bilde / lag en figur som du ønsker å flytte på**
    <details>
      <summary>👈 Instruksjoner for å legge til et bilde i canvas</summary>

    1) Lagre bildefilen i samme mappe som `.html` filen
    2) Legg til denne linja rett over `<canvas>`:
    ```html
    <img id="image" src="">
    ```
    3) Endre det inni fnuttene til `src=""` til navnet på bildefilen. Eksempel:
    ```html
    <img id="image" src="terje.png">
    ```
    - Bildet burde vise seg på skjermen!
    4) Legg til denne linja rett over `// Skriv kode her!`:
    ```js
    const img = document.getElementById('image')
    ```
    5) Lag en funksjon som tegner bildet, og kjør den når siden lastes
        - Lag en funksjon `draw()` og tegn bildet med [`ctx.drawImage()`](https://www.w3schools.com/tags/canvas_drawimage.asp):
        ```js
        function draw() {
            ctx.drawImage(img,0,0)
        }
        ```
        - Skriv denne linja en plass utenfor funksjonene (globalt)
        ```js
        window.onload = draw()
        ```
        Denne linjen kjører tegnefunksjonen så fort alt er lastet inn, så vi kan forsikre oss om at bildet laster inn ordentlig.
        - Bildet burde vise seg i canvas!
    6) Evt. legg til en `style="display:none;"` for å gjemme bildet på utsiden av canvas:
    ```html
    <img id="image" src="terje.png" style="display:none;">
    ```
      
    </details>
2) **Lag variabler for posisjonen til bildet/figuren**
    - Eks. `let x`, `let y`
    - Sett de på riktig plass
    - Hvis du har gjort det riktig, så skal du kunne endre på variablene, og posisjonen på canvas endrer seg!
3) **Lag en funksjon som tegner opp bildet/figuren**
   - Kall den noe som `draw()` (om du ikke har gjort det allerede)
   - Legg til en `requestAnimationFrame(draw)` i bunnen av funksjonen
   - Gjerne også legg til en `ctx.clearRect(0,0,c.width,c.height)` på toppen
   
   <details>
   <summary>👈 Sjekk om du har skrevet riktig så langt</summary>
   
   ```html
   <body>
   <img id="image" src="terje.png" style="display:none;">
   <canvas id="canvas" width="800" height="600"></canvas>
   
   <script>
   const c = document.getElementById('canvas');
   const ctx = c.getContext('2d');
   const img = document.getElementById('image');
   // Skriv kode her! ↓↓↓
   let x = 0;
   let y = 0;
   
   function draw() {
       ctx.clearRect(0,0,c.width,c.height)
       ctx.drawImage(img,x,y)

       requestAnimationFrame(draw)
   }
   window.onload = draw()
   
   </script>
   </body>
   ```
   </details>
4) **Flytte med tastetrykk!**
    
    For å flytte med tastetrykk, så må vi få nettleseren til å "høre etter" tastetrykk som vi gjør. Da kan vi bruke noe som heter [`addEventListener`](https://www.w3schools.com/jsref/met_document_addeventlistener.asp)!
    1) Lim inn denne koden rett under variablene:
    ```js
    function handleKeyDown(e){
        if (e.code === "ArrowDown") y += 1
    }
    document.addEventListener("keydown", handleKeyDown)
    ```
    - Hvis vi trykker på Pil Ned tasten nå, så burde bildet/figuren flytte seg nedover!
5) **Gjøre bevegelsen mer *smooth***
    
    Nå beveger bildet/figuren seg, men hvis man holder ned tasten så vil man se at det tar litt tid før den begynner å bevege seg; samt så er ikke bevegelsen veldig *smooth*. Problemet er at bevegelsen oppdaterer seg med tastaturet - og det vi vil er at den skal bevege seg så lenge tasten er trykket ned. Dette er to forskjellige ting.
    1) Vi kan bruke noen flere variabler som lagrer på om en knapp er trykket ned eller ikke
        - Lag en variabel som heter `moveDown` og sett den til `false`.
        - I funksjonen `handleKeyDown`, bytt ut `y += 5` med `moveDown = true`
        - I `draw()`-funksjonen, legg til denne `if`-sjekken:
        ```js
        if (moveDown) y += 1
        ```
        Nå burde figuren/bildet bevege seg nedover i det vi trykker på tasten - men vi har et nytt problem hvor `moveDown` aldri blir satt til false. Vi må registrere at vi *slipper opp* tasten, i motsetning til at vi trykker en knapp *ned*.
        - Vi legger til en funksjon som heter `handleKeyUp` - denne er identisk til `handleKeyDown`, bare at den setter `moveDown` til `false`. Deretter legger vi til en `addEventListener` for denne:
        ```js
        function handleKeyUp(e){
            if (e.code === "ArrowDown") moveDown = false
        }
        document.addEventListener("keyup", handleKeyUp)
        ```
        - Nå burde bildet/figuren bevege seg nedover og slutte å bevege seg i det vi slutter å holde Pil Ned!
    <details>
    <summary>👈 Sjekk om du skrev det riktig!</summary>

    ```js
    const c = document.getElementById('canvas');
    const ctx = c.getContext('2d');
    const img = document.getElementById('image')
    // Skriv kode her! ↓↓↓

    let x = 0;
    let y = 0;
    let moveDown = false;
    
    function handleKeyDown(e){
        if(e.code === "ArrowDown") moveDown = true;
    }
    
    function handleKeyUp(e){
        if(e.code === "ArrowDown") moveDown = false;
    }
    
    document.addEventListener("keydown", handleKeyDown)
    document.addEventListener("keyup", handleKeyUp)
    
    function draw() {
        ctx.clearRect(0,0,c.width,c.height)
        
        if(moveDown) y += 1
        
        ctx.drawImage(img,x,y)
        requestAnimationFrame(draw)
    }
    window.onload = draw()
    ```
    </details>
    <br>

6) **Flytte i flere retninger!**
    
    Nå kan vi bare bevege oss i én retning - nedover. Hvis vi skal bevege oss i en annen retning, som f.eks. til høyre - så kan vi følge samme oppskrift som i punkt 5!
    1) `let moveRight = false`
    2) Legg til i `handleKeyDown()`:
    ```js
    function handleKeyDown(e){
        if (e.code === "ArrowDown") moveDown = true
        if (e.code === "ArrowRight") moveRight = true
    }
    ```
    3) I `draw()`-funksjonen:
    ```js
    function draw() {
        ctx.clearRect(0,0,c.width,c.height)
        ctx.drawImage(img,x,y)

        if(moveDown) y += 1
        if(moveRight) x += 1

        requestAnimationFrame(draw)
    }
    ```
    4) I `handleKeyUp()`
    ```js
    function handleKeyUp(e){
        if(e.code === "ArrowDown") moveDown = false
        if(e.code === "ArrowRight") moveRight = false
    }
    ```

    Du burde nå kunne flytte bildet/figuren både nedover og til høyre!

**Oppgave 2: BONUS!**
- Lag resten av retningene mulig også!
- Prøv å lag vegger rundt canvas, ikke få bildet/figuren til å rømme :D
- I Økt 2 lagde Terje et eksempel hvor han fikk bakgrunnen til å bevege seg i [demo8.html](./eksempler/økt%202/demo8.html) - prøv å få veien til å bevege seg nedover!