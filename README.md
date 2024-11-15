# 2.-Ariketa-IKT-HTML
<!DOCTYPE html>
<html>
<head>
    <meta charset='utf-8'>
    <meta http-equiv='X-UA-Compatible' content='IE=edge'>
    <title>MMA</title>
    <style>
        body { background-color: #00ffead8 ; font-family: Arial, sans-serif; color: #141212dd}
        h1 { text-align: center; color: #0f7c79; }
        h2 { font-size:20px; color: black}
        nav a { margin-right:20px; font-weight: bold; text-decoration: none; color:#0f7c79 }
        /* Testua ezkerretik eskuinera mugitzen */
        .mugimendua {
            position: relative;
            animation: mugituEzkerEskuin 5s infinite linear;
        }

        @keyframes mugituEzkerEskuin {
            from { left: 0; }
            to { left: 100%; }
        }
    </style>
    <meta name='viewport' content='width=device-width, initial-scale=1'>
    <link rel='stylesheet' type='text/css' media='screen' href='main.css'>
    <script src='main.js'></script>
    
</head>
<body>
    <h1><span style="color: rgb(255, 0, 0);">MMA</span></h1>
    <nav>
        <a href="Lehenengoa.html">Historia</a>
        <a href="Bigarrena.html">Jokalariak</a>
        <a href="Hirugarrena.html">Lehiaketa</a>   
    </nav>
    </div>
    <h2>Audioa</h2>
    <audio controls>
        <source src="./Audioa.mp3" type="audio/mpeg">
        Zure nabigatzaileak ez du audioa erakusten.
    </audio>
    <p class="mugimendua">SheralGOAT BECKER</p>

</body>
</html>
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ping Pong en un Cuadro</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        /* Contenedor con borde */
        #game-container {
            width: 820px;  /* Un poco más ancho que el canvas */
            height: 420px; /* Un poco más alto que el canvas */
            border: 5px solid #FFF;
            background-color: #333;
            display: flex;
            justify-content: center;
            align-items: center;
            margin: 20px auto;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.5);
        }
        /* Canvas del juego */
        canvas {
            background-color: #000;
        }
    </style>
</head>
<body>
    <div id="game-container">
        <canvas id="pong" width="800" height="400"></canvas>
    </div>
    <script>
        const canvas = document.getElementById('pong');
        const context = canvas.getContext('2d');

        // Crear el jugador y la IA
        const paddleWidth = 10, paddleHeight = 100;
        const player = {
            x: 0,
            y: canvas.height / 2 - paddleHeight / 2,
            width: paddleWidth,
            height: paddleHeight,
            color: '#FFF',
            score: 0
        };
        const ai = {
            x: canvas.width - paddleWidth,
            y: canvas.height / 2 - paddleHeight / 2,
            width: paddleWidth,
            height: paddleHeight,
            color: '#FFF',
            score: 0
        };

        // Crear la pelota
        const ball = {
            x: canvas.width / 2,
            y: canvas.height / 2,
            radius: 10,
            speed: 5,
            velocityX: 5,
            velocityY: 5,
            color: '#05EDFF'
        };

        // Dibuja un rectángulo (para las paletas y la red)
        function drawRect(x, y, w, h, color) {
            context.fillStyle = color;
            context.fillRect(x, y, w, h);
        }

        // Dibuja un círculo (para la pelota)
        function drawCircle(x, y, r, color) {
            context.fillStyle = color;
            context.beginPath();
            context.arc(x, y, r, 0, Math.PI * 2, false);
            context.closePath();
            context.fill();
        }

        // Dibuja el texto (para las puntuaciones)
        function drawText(text, x, y, color) {
            context.fillStyle = color;
            context.font = "45px sans-serif";
            context.fillText(text, x, y);
        }

        // Dibuja la red
        function drawNet() {
            for (let i = 0; i <= canvas.height; i += 15) {
                drawRect(canvas.width / 2 - 1, i, 2, 10, '#FFF');
            }
        }

        // Dibuja todo el juego
        function render() {
            drawRect(0, 0, canvas.width, canvas.height, '#000'); // Fondo
            drawNet();
            drawText(player.score, canvas.width / 4, canvas.height / 5, '#FFF');
            drawText(ai.score, 3 * canvas.width / 4, canvas.height / 5, '#FFF');
            drawRect(player.x, player.y, player.width, player.height, player.color);
            drawRect(ai.x, ai.y, ai.width, ai.height, ai.color);
            drawCircle(ball.x, ball.y, ball.radius, ball.color);
        }

        // Detecta la colisión entre la pelota y las paletas
        function collision(b, p) {
            b.top = b.y - b.radius;
            b.bottom = b.y + b.radius;
            b.left = b.x - b.radius;
            b.right = b.x + b.radius;
            
            p.top = p.y;
            p.bottom = p.y + p.height;
            p.left = p.x;
            p.right = p.x + p.width;
            
            return b.right > p.left && b.bottom > p.top && b.left < p.right && b.top < p.bottom;
        }

        // Restablece la pelota después de un punto
        function resetBall() {
            ball.x = canvas.width / 2;
            ball.y = canvas.height / 2;
            ball.velocityX = -ball.velocityX;
            ball.speed = 5;
        }

        // Actualiza la posición de los elementos
        function update() {
            ball.x += ball.velocityX;
            ball.y += ball.velocityY;
            
            // IA simple para la paleta
            ai.y += (ball.y - (ai.y + ai.height / 2)) * 0.1;

            // Rebote de la pelota en el borde superior e inferior
            if (ball.y + ball.radius > canvas.height || ball.y - ball.radius < 0) {
                ball.velocityY = -ball.velocityY;
            }

            let playerPaddle = (ball.x < canvas.width / 2) ? player : ai;

            // Colisión con la paleta
            if (collision(ball, playerPaddle)) {
                let collidePoint = ball.y - (playerPaddle.y + playerPaddle.height / 2);
                collidePoint = collidePoint / (playerPaddle.height / 2);

                let angleRad = (Math.PI / 4) * collidePoint;
                let direction = (ball.x < canvas.width / 2) ? 1 : -1;
                ball.velocityX = direction * ball.speed * Math.cos(angleRad);
                ball.velocityY = ball.speed * Math.sin(angleRad);
                
                ball.speed += 0.5;
            }

            // Actualiza el puntaje y restablece la pelota
            if (ball.x - ball.radius < 0) {
                ai.score++;
                resetBall();
            } else if (ball.x + ball.radius > canvas.width) {
                player.score++;
                resetBall();
            }
        }

        // Mueve la paleta del jugador
        canvas.addEventListener("mousemove", evt => {
            let rect = canvas.getBoundingClientRect();
            player.y = evt.clientY - rect.top - player.height / 2;
        });

        // Llama a la función de renderizado y actualización
        function game() {
            update();
            render();
        }

        // Ejecuta el juego a 50 fotogramas por segundo
        const fps = 50;
        setInterval(game, 1000 / fps);
    </script>
</body>
</html>
