## Økt 2
- Horseracer!

(Løs detta med variabler og if-setninger!)
1) Få hesteridern inn i canvas!    
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
            background-image: url("https://cdn.discordapp.com/attachments/1440323879951269959/1442494765580157129/image.png?ex=6925a370&is=692451f0&hm=03b40f92588bba245e135ebbacf23500f87351299c3245a79cd1e3abcaab6ff7&");
        }
    </style>
</head>
<body>
    <canvas id="vanGogh" width="800" height="600"></canvas>
    <img src="https://cdn.discordapp.com/attachments/1440323879951269959/1442501071216971908/horseracer.jpeg?ex=6925a94f&is=692457cf&hm=b320d2bde2ff7be1261f7eb9cd6388611e6376affee242ec9f5e721f49abb11f&" id="horse">
    <script>
        const c = document.getElementById('vanGogh');
        const ctx = c.getContext("2d");
        const img = document.getElementById('horse');
        
        function move() {
            ctx.clearRect(0, 0, c.width, c.height);
            // Skriv her (!)

            
            requestAnimationFrame(move)
        }
        move();

        function checkWalls() {
            if(x > c.width) {
                return -xSpeed
            } else if (x <= 0) {
                xSpeed = 5;
                return xSpeed;             
            }
        }

    </script>
</body>
</html>
```