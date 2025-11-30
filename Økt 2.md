## Økt 2
### Oppgave 1: Variabler
1) Lag to [variabler](https://www.w3schools.com/js/js_variables.asp) som skal tilsvare x og y posisjonen til en sirkel - sett de til en valid verdi 
    - `let x`
    - `let y` 
2) Bruk disse variablene for å lage en sirkel med `arc()`
3) Endre verdiene i `x` og `y` og lagre etter hver endring
    - Du burde se sirkelen flytte på seg!

### Oppgave 2: Endre størrelse
En [`arc()`](https://www.w3schools.com/tags/canvas_arc.asp) sin tredje verdi står for `radius`:
```js
ctx.arc(x, y, radius, startAngle, endAngle)
```
1) Lag enda en variabel for størrelsen - sett den til en valid verdi
    - `let size`
2) Sett variablen inn på riktig sted i `arc()`
3) Endre verdien i `size` og lagre etter hver endring
    - Sirkelen burde endre størrelse!

### Oppgave 3: Tegn en sirkel ved bruk av en funksjon
Til nå så kjører koden vi skriver fra topp til bunns. Men ved hjelp av [**funksjoner**](https://www.w3schools.com/js/js_functions.asp) så kan vi velge hvor og når deler av koden skal kjøre!

1) Lag en funksjon som heter `move()`.
    - `function move(){}`
2) Mellom krøllparantesene (`{}`) kan vi trykke på *Enter* for et linjeskift
3) Fortsatt mellom krøllparantesene, så kan vi flytte vår sirkel:
```js
function move() {
    ctx.arc(x, y, size...)
    ctx.stroke()
}
```
4) Skriv en "`move()`" på linja *over* `function move()`
5) Sirkelen burde vise seg på skjermen!

### Oppgave 4: Få sirkelen til å flytte på seg!
JavaScript har en innebygd funksjon som vi kan bruke for å lage en "animasjon" som heter [`requestAnimationFrame()`](https://www.w3schools.com/jsref/met_win_requestanimationframe.asp).

For å kjøre denne, så må vi sende med funksjonen vi laget!

1) Legg til en `requestAnimationFrame(move)` nederst i `move()` funksjonen.
    - Vi trenger navnet på funksjonen vi laget inne i parantesene på `requestAnimationFrame`!

Nå *animerer* vi teknisk sett, men vi må spesifisere hva som skal *endre* seg i `move()`-funksjonen.

2) Legg til en `x += 1` -  en linje over `requestAnimationFrame`
    - Sirkelen burde nå bevege seg horisontalt! Men som en svart stripe...
3) Helt i toppen av funksjonen, så burde vi "cleare" canvas før vi beveger oss. Legg til denne linja i toppen av `move()`:
```js
function move() {
    ctx.clearRect(0, 0, ctx.width, ctx.height)
    ...
```
- Du burde nå ha en sirkel som beveger seg horisontalt over skjermen!
    - Hvis det fortsatt er en svart stripe, så kan det hende du burde putte [`ctx.beginPath()`](https://www.w3schools.com/tags/canvas_beginpath.asp) rett over `ctx.arc()`.
4) Prøv selv!
    - Nå beveger sirkelen seg horisontalt, hvordan skal man få den til å bevege seg vertikalt?

### Oppgave 5: Få sirkel til å holde seg innenfor canvas
Nå forsvinner sirkelen ut av canvas, som kan være ukjekt - fordi vi helst vil se sirkelen...

Da må lage noen [`if`](https://www.w3schools.com/jsref/jsref_if.asp)-sjekker for å sjekke om sirkelen er der den skal være!

1) Legg til en ny variabel for fart sammen med de andre variablene
    - `let speed`
2) Endre `x += 1` til `x += speed`
    - Hvis `speed` er satt til `1`, så vil du få samme resultat!
3) Inne i funksjonen, lag en ny linje hvor du skriver "`if()`".
    - Skriv inni parantesene:
    - `x > c.width`
    - Her sier vi: **Hvis x-positionen til sirkel er mer enn bredden på canvas**
4) Etter denne `if`-en, så kan vi skrive:
    - `speed = -speed`
    - Dette betyr: **Sett fart til negativ fart** (som da er motsatt retning)
5) Sirkel burde sprette i motsatt retning!

**Bonus:**
- Nå går deler av sirkelen ut av rammen - **prøv å få hele sirkelen til å holde seg innenfor**! (Hint: Her må vi kanskje gjøre noe med `c.width`)
- Prøv å gjør det samme som vi har gjort, men vertikalt i stedet for horisontalt!

---

## Bonusoppgave!

### Horseracer!

(Løs detta med variabler og if-setninger! Ta utgangspunkt med koden under som mal!)
1) Få hesteridern inn i canvas! ([`drawImage()`](https://www.w3schools.com/graphics/canvas_images.asp))   
2) Få hesteridern til å bevege seg horisontalt (variabler)
3) Få den til å bounce vekk fra høyre vegg
4) Få den til å bounce vekk fra venstre vegg også (Reset x verdi!)
5) Få den til å gå diagonalt (da trenger vi kanskje tak og gulv?)
6) Trekk ut vegg-sjekk i egen funksjon (return)

BONUS BONUS!
- FÅ ridern til å bevege seg langs kantene med klokka

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
            background-image: url("https://kompis.s-ul.eu/zHiH1V9G");
        }
    </style>
</head>
<body>
    <canvas id="vanGogh" width="800" height="600"></canvas>
    <img src="https://kompis.s-ul.eu/hJ5YVKvY" id="horse">
    <script>
        const c = document.getElementById('vanGogh');
        const ctx = c.getContext("2d");
        const img = document.getElementById('horse');
        
        move();
        function move() {
            ctx.clearRect(0, 0, c.width, c.height);
            // Skriv her (!)

            
            requestAnimationFrame(move)
        }

    </script>
</body>
</html>
```